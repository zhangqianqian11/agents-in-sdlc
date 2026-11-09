---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，您让 Copilot 云智能体处理了三个与改善用户体验和添加功能有关的任务。云智能体会异步自主工作，但监控这些任务仍然很重要。

您可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，您可以查看所有拥有未关闭拉取请求的智能体任务、了解它们执行的操作，还可以指导正在运行的会话。

本课将学习如何：

- 使用智能体页面监控云智能体任务。
- 指导正在运行的会话，要求添加其他功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队意识到也可以同时添加浅色模式。网站正在更新样式和切换功能，因此一并实现浅色模式很合理。现在，您要指导智能体，让它在高对比度模式之外也添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态：

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包含您分配给 Copilot 的任务，例如：
    - 为代码库补充文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的步骤，例如：
    - 检出存储库代码。
    - 为 Copilot 创建环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 同时查看右侧显示的拉取请求面板，可在此查看 PR 及其更改的文件，以便进一步监控。

## 指导云智能体

现在，向正在实现高对比度模式的 Copilot 会话提出要求，让它同时添加浅色模式。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始处理任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示“Steer active session while Copilot is working”对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认提示并将它纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务。完成当前步骤后，它会将新要求纳入工作流程。

这可能还需要几分钟。您可以利用这段时间回顾到目前为止的学习内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和指导 GitHub Copilot 云智能体任务的中心位置。在这里，您可以跟踪所有进行中和已完成的任务、审查正在执行的工作，还可以调整正在运行的任务范围或提供额外指导。

您学习了以下内容：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 指导正在运行的会话，要求添加其他功能。

Copilot 正在处理无障碍功能。接下来，前往[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
