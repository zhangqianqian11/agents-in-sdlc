---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** 可在云端异步运行。您可以在 GitHub 上分配任务，云智能体便会在后台探索存储库、进行更改并创建拉取请求，让您继续处理其他工作。

在本系列练习中，您将添加云智能体要遵循的自定义指令，然后分配 GitHub issue 并让它实现任务。您还会查看并使用自定义智能体来引导工作方式，在智能体仪表板中监控和指导会话，最后审查云智能体创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 准备工作][ex0] | 设置 | 创建存储库和 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云智能体要遵循的自定义指令 |
| [2. 云智能体][ex2] | 异步智能体 | 将 issue 分配给 Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 查看并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并指导智能体会话 |
| [5. 迭代改进][ex5] | 审查 | 审查 PR、迭代改进 Copilot 的工作并选择后续步骤 |

## 先决条件

参加本研讨会前，请确保您具备以下条件：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 方案的 GitHub 帐户
- [ ] 已为您的帐户或组织启用 Copilot 云智能体

> [!TIP]
> 没有付费方案？经过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 方案包含本研讨会使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可用它完成全部课程。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot 云智能体适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，不适用于 Copilot Free。使用 Copilot Business 或 Enterprise 时，管理员必须为组织启用此功能。

> [!NOTE]
> 本课程不介绍 MCP。若要了解如何将 MCP 服务器与 Copilot 配合使用，请参阅 [CLI 课程](../cli/)或 [VS Code 课程](../vscode/)。

## 开始

**[从练习 0：准备工作开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
