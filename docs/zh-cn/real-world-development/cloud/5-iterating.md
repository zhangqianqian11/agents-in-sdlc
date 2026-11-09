---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，您让 GitHub Copilot 完成了多个改善用户体验和添加功能的任务，包括为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在，请检查代码更改；必要时向 Copilot 提供反馈，改进其工作。

### 场景

即使加入生成式 AI，软件设计和 DevOps 的基本原则也不会改变。我们仍然需要审查生成的代码，并遵循正常的 DevOps 流程。接下来，在为团队其他成员启用审查前，先审查 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## 安全与 GitHub Copilot 云智能体

Copilot 云智能体会异步、无人监督地执行任务，因此设置了一些安全限制：

- Copilot 只能读取存储库，并且只能写入它用于提交代码的分支。
- 云智能体在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR）：为代码添加文档。您将使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。您也可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 在浏览器中打开 GitHub 存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会显示 **[WIP]**。请等待 Copilot 完成，这可能需要几分钟。

4. 准备就绪后，选择 **Files changed** 选项卡，审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 检查新增的 TSDoc 文档注释及其他文档。具体更改可能各不相同。留意 TSDoc 文档注释、TypeScript 编码约定和文件注释头；这些要求来自您在练习 2 开始时查看的自定义指令文件。
6. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。如果一切正常，单个 Astro app 的项目检查应全部通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。您可以像在 PR 中提及其他团队成员一样，在评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新会话来处理请求。由于结果具有非确定性，我们无法提供固定的提示文本。您可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro app 的结构。

1. 添加评论并提及 **@copilot**，请求它修改生成的文档。可以使用上述建议，也可以提出其他文档方面的要求。
2. 选择 **View Session**，观察 Copilot 执行更新。请注意 Copilot 会启动新会话。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡，审查更改。

您可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为准备审查状态，然后合并。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来，查看 Copilot 为解决“在游戏详细信息页显示相关游戏”问题而创建的 PR。

1. 在 GitHub.com 中打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准。
7. 如果有，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查区域中排队运行。如果一切正常，单个 Astro app 的项目检查应全部通过。此过程可能需要几分钟。
9. **可选：** 您可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已完成相关游戏功能。您可以继续迭代并要求云智能体更新，例如调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（注意，分配此任务时，您尚未更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中发表评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查无障碍自定义智能体实现的功能。此 PR 应同时包含您在练习 3 中分配的高对比度模式，以及在练习 4 中通过任务控制中心要求的浅色模式。

1. 在 GitHub.com 中打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会显示 **[WIP]**。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，尤其注意：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。如果一切正常，单个 Astro app 的项目检查应全部通过。此过程可能需要几分钟。
10. **可选：** 您可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式，并确认页面重新加载后设置仍然有效。

请注意，无障碍自定义智能体如何帮助 Copilot 遵循无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可像之前一样在评论中提及 **@copilot** 并要求更新。

## 可选练习 — 继续委派任务

云智能体适合处理待办事项，让您腾出时间专注其他工作。可以在自己的存储库中创建更多 issue 并分配给 Copilot，练习这一工作方式。例如：

- 在游戏详细信息页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

您已完成云智能体课程。在这些课程中，您：

- **检查了存储库自带的自定义指令文件**，以便之后了解它们如何影响云智能体的输出。
- **将 issue 分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**，实现无障碍功能以及高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为任务控制中心**，监控并指导无障碍会话。
- **审查并迭代改进云智能体的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 审查与后续步骤

云智能体课程到此结束。若要继续探索，可学习其他课程：

- 🖥️ **[VS Code 课程](../../vscode/)** — 在 IDE 中体验 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 课程](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖率 issue 分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
