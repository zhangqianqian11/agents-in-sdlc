---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot 在您的计算机上实时工作，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。您可以查看内联差异、逐项接受或拒绝更改，也可以继续发送提示来调整或扩展工作，全程无需离开编辑器。

本练习将学习如何：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中指导智能体，在高对比度模式之外添加浅色模式切换。

## 场景

您在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了额外需求：为觉得默认主题太暗的用户添加浅色模式切换。您将指导现有会话扩展工作，而不是从头开始新对话。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天记录，查看工具调用、文件读取、文件编辑和终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中智能体修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准您认可的命令。

5. 等待智能体完成高对比度模式实现。完成后，它会显示更改摘要。

## 指导智能体添加浅色模式

高对比度模式完成后，继续在同一对话中添加浅色模式切换。由于仍处于同一个聊天会话，智能体会保留先前工作的上下文，包括修改的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会检查现有更改，并提出添加浅色模式的修改。
3. 像审查高对比度功能一样，逐项检查新的编辑，并使用 **Keep** 或 **Undo** 进行处理。
4. 如果智能体的方向不符合预期，例如试图以您不喜欢的方式重构切换 UI，可选择聊天输入框旁的 **Stop** 按钮，然后发送澄清提示。

> [!TIP]
> 使用后续提示指导正在运行的会话，比从头开始更快。可以要求调整（“简化 CSS，改用 CSS 变量，避免重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式集成到现有设置面板”），或扩展范围（“为两个开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的全部文件。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开部分文件并检查差异。确认自定义智能体提出的无障碍模式已反映在实现中，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

您使用 Copilot Chat 智能体模式监控并指导了正在进行的会话，审查每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课中，您学习了：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 通过后续提示在会话进行中指导智能体。

在[下一课][next-lesson]中，您将以拉取请求形式发布无障碍功能，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
