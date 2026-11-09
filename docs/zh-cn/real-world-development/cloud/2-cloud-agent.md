---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而积压的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建待审查的草稿 PR，供开发人员检查。这有助于分担繁琐任务、加快开发流程，让开发人员专注于更重要的工作。

本课将使用 Copilot 云智能体了解以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初承包网站开发的团队没有妥善编写文档，甚至完全没有文档。首先，团队希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们希望在每款游戏的详细信息页上显示相关游戏（即同一类别中的其他游戏），让用户可以继续浏览。目前不要求实现完整体验，只需提供可用于验收测试的初步实现。此项工作目前受阻，但其他 issue 优先级更高。

这两项都是容易被推迟的任务，适合交给 Copilot 云智能体。它可以异步处理任务，让开发人员先处理其他工作，再回来审查 Copilot 的结果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。您可以通过多种方式启动工作，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。您可以在任务进行中或完成后查看 Copilot 会话。解决方案准备好审查时，Copilot 会在拉取请求中提及您。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，您仍然需要思考问题。即使看似简单的操作也可能包含复杂细节，因此[应仔细限定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

之前的练习介绍了自定义指令及其引导 Copilot 的方式。如果您已完成自定义指令练习，就已看到 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配任务前，先只读查看存储库已有的指令文件，以便稍后识别其影响。

在 GitHub 网页界面或已启动的 Codespace 中查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` issue 分配给云智能体时，留意这些指令文件如何影响 PR 中的 TSDoc 注释、TypeScript 约定和文件注释头。稍后的课程还会再次检查这些内容。

## 为 Copilot 云智能体设置开发环境

代码开发通常需要专用环境和设置脚本，以确保环境就绪。将任务分配给 Copilot 时也一样，它会像软件工程师一样执行任务。

云智能体在 GitHub Actions 中使用其环境。您可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]，在 Copilot 开始工作前设置所需的开发工具和依赖项。本实验已提前配置好工作流，其中包含 Node.js、项目依赖项、端到端测试所需的浏览器，以及这个单体 Astro app 的迁移和填充后的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许您从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动为您克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端 / app 设置 - Node.js（整个 app 现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器可让您从存储库的 **Actions** 选项卡手动运行工作流，以便测试配置，而不必等待 Copilot 启动。

## 添加文档

大多数项目都存在文档过时或缺失的问题，这类技术债务会降低工作效率，也会增加维护代码或帮助新成员加入团队的难度。Copilot 擅长创建文档，因此这项任务适合交给云智能体在后台处理。稍后的练习会审查它的工作。

1. 在新的浏览器标签页中打开 GitHub 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的 PR。标题可能类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能创建待办事项列表，因为它需要创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 查看 Copilot 即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理 issue 的更新。
15. 选择 **View session**。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口，才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解状态、暂停思考并制定方案，然后生成代码。

此过程可能需要几分钟。云智能体的主要优势之一是异步执行任务，让我们可以专注于其他工作。现在也将第二项任务分配给云智能体，然后自己编写代码，为应用添加功能。

## 在游戏详细信息页添加相关游戏部分

云智能体的一大优势是可以拆分工作：您处理一项任务，它处理另一项任务。为设计团队添加相关游戏部分可能不会花很长时间，但仍可将这段时间用于其他工作。现在将任务分配给云智能体。

1. 返回 GitHub 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给 issue](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，您会在 issue 的第一条评论中看到 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它会像软件工程师一样工作，因此您无需一直监控，可以等任务完成后再审查结果。现在继续创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。您可以将 issue 分配给云智能体，让它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

您学习了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定 issue 范围的重要性。
- 将 issue 分配给 Copilot。

云智能体正在后台工作，现在可以开始创建和使用自定义智能体。[云智能体也支持 MCP 服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
