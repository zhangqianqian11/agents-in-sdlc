---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行测试。开发人员需要找到相关文件、进行更改、运行测试、调试、找出遗漏的文件并继续更新。Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式可在 IDE 中自主工作。它会先探索项目结构、执行必要的更新、运行测试等任务，并自动修复发现的问题。现在，使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同的体验。智能体模式在 IDE 中执行任务，可快速反馈和交互；云智能体则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot 智能体模式探索项目、找到相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将更改合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，您希望用户能够按类别和发行商筛选。上一课已经添加了发行商辅助函数；现在将使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

先探索网站，了解现有功能。

这是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。网站是单个 Astro app，页面在构建时生成为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过标准 DOM API 和范围受限的 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行：

1. 返回上一课结束后所在的 `main` 分支。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到筛选工作分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端的 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是托管 Web 服务器的 codespace 私有隧道。

### 浏览网站

网站主要功能包括：

- **主页**：显示桌游列表、标题、图片和说明。
- **游戏详细信息页**：选择游戏后可查看标题、说明、发行商和类别等详细信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型选择因练习而异；每一步会在相关位置说明。

网站已有基础功能，但还可以添加更多能力。先查看待办事项。您从模板创建项目时，系统已自动创建 issue；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体可以跨文件规划和实现更改、运行终端命令并调用工具。

   ![Chat 视图中智能体选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。构建本研讨会时使用了 Claude Sonnet 4.5，因此将其列入说明，以便建议相对一致。但由于 LLM 具有概率性，收到的建议可能与示例不同，这是正常现象。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，查看 issue 待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有 issue 的命令。
5. 查看 Copilot 生成的 issue 列表及其优先级建议。

## 查看指令文件

生成代码前，先查看用于向 Copilot 提供上下文的指令文件。项目的[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件包含向网站添加功能时的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - Astro、Tailwind CSS 和 Drizzle 等文件类型的专用指令链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，便于重用、拆分大型指令集，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三类更新：

- 添加或改进数据层（`src/lib/`）中的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以帮您完成这些工作。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中仍然选中 **Agent**。
4. 确认仍然选择 **Claude Sonnet 4.5**。
5. 使用与待办 issue 相关的要求提示 Copilot：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关数据层辅助函数、UI 和测试。然后它会修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，可能需要几分钟。过程中编辑器可能会出现 **Keep** 和 **Undo** 按钮。工作完成后，每项更改都会提供 **Keep** 或 **Undo**，无需在智能体工作时提前选择。

7. Copilot 提示时选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 工作期间可能会暂停，或暂时出现测试失败。这很正常。它会反复生成代码并运行测试，直至任务完成且未发现错误。

   ![完整 Copilot 智能体模式聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜，您已使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。本课中，您学习了如何：

- 使用智能体模式跨 UI 和数据层实现新功能。
- 让 Copilot 探索项目、找到相关文件并协调更改。
- 在合并到代码库前审查 Copilot 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 附加探索练习 — 实现分页

随着游戏列表扩大，需要启用分页。使用本课所学内容提示 Copilot 实现分页。请考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 决定分页方式，例如是否允许用户选择页面大小，或使用固定大小。
- 在提示中提供 Copilot 所需的指导，确保实现符合预期。
- 可能需要通过提供上下文和要求更改来迭代。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
