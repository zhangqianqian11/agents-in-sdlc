---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务有特定要求，就应让 Copilot 能够访问相关指导。[指令文件][instruction-files]可提供代码生成指导。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 在现有指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的全局标准。
- 再次发送相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选代码切片提交到 `main`，供下一课中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能无法完全符合所有标准。Copilot 的输出具有非确定性。本练习的重点是观察更新指令前后的行为趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代风格。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加注释块，说明文件用途。

您将使用指令文件，确保 Copilot 获取正确的信息并按照这些实践执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解您的编码风格和要求。可以指定编码约定、库和注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给 Copilot 的单个存储库级指令文件，提供大多数请求都适用的项目信息。
- `.github/instructions/*.instructions.md` 可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体会在生成代码时使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目说明、目录结构和全局编码标准。
- 使用 `*.instructions.md` 针对文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保说明清晰，并提供代码示例。

指令文件没有唯一正确的写法，可以通过实践找到适合项目的方法。

> [!TIP]
> 使用 GitHub Copilot 的项目都应具备可靠的指令文件集合。查看本项目时，您会发现适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令文件。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本课程会在合适位置介绍具体方式。
>
> 可在 [awesome-copilot][awesome-copilot] 中查找指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目的自定义指令文件

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。
3. 打开 `.github/instructions` 文件夹，查看其中适用于 Astro、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段使用 glob 确定指令适用的文件，例如符合 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看该文件中针对单元测试的具体指导。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部链接。它会指向其他指令文件和项目现有文件，便于拆分大型指令集并复用示例。（这些路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `.github/copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来会添加 TSDoc 注释和文件注释头的要求。

## 为更改创建分支

按照最佳实践，为本次更改创建分支。

1. 返回上一课中使用的 Codespace。
2. 在 Codespace 中打开新终端，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型选择因练习而异；每一步会在相关位置说明。

为了观察自定义指令的作用，先在当前指令下发送提示，之后更新指令并再次发送相同提示。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数和测试。
6. 注意生成的辅助函数是带类型的函数，第一个参数是 `db` 客户端，返回带类型的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使未明确要求，也可能添加文档注释。若出现这种情况也没关系；本练习仍要观察更新指令后的一致性改进。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级指导。添加编码标准可以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分。它记录了项目编码标准，但尚无代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 存储库级 `.github/copilot-instructions.md` 适合放置项目级标准；也可以通过 `*.instructions.md` 为特定语言、文件类型或任务提供指导。

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除当前对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

您刚才引导 Copilot 遵循新的项目标准，并将其应用到后续练习会用到的实际代码中。

## 提交指令并推送分支

指令文件与其他项目资产一样，通过源代码管理进行维护。现在将分支推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改均已保存：

   ```bash
   git status
   ```

3. 暂存并提交指令更新和 Copilot 为基础辅助函数修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并将其关联到待办事项中的文档标准 issue。可以手动操作，也可以让已连接项目工具的 Copilot 查找 issue、创建 PR 并合并。

> [!NOTE]
> 这是 Copilot 首次代表您操作 GitHub。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示您登录 GitHub。按照提示授权即可。后续课程会介绍此连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的 issue，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找 issue 并创建 PR。
5. 如出现授权提示，请先审查 Copilot 要执行的 GitHub 操作，再选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如出现授权提示，请先审查 GitHub 操作和 shell 命令，再选择 **Allow**。

您已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

您了解了 Copilot 如何获取指令文件中的上下文，并在 VS Code 中使用 Copilot Chat：

- 使用代码生成提示观察 Copilot 在*现有*指令下的输出。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次发送相同提示，观察建议的代码采用了新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一课使用。

接下来，您将使用[智能体模式][next-lesson]，让 Copilot 跨多个文件实现新功能。

## 资源

- [GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的五个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
