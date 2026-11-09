---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

您已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。您：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选 issue。
- 添加星级评分，在转发浏览器中审查结果，并手动合并第一个拉取请求（PR）。
- 从筛选 issue 开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下审查。
- 使用自定义指令指导智能体，自定义现有 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于管理上下文、模型、共享会话和可选云端委派的斜杠命令。

## 您交付的内容

本研讨会包含两个 PR 里程碑，分别从更新后的 `main` 创建独立分支：

1. **星级评分：** 在游戏卡片上显示已有 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：** 实现筛选，更新指令并将其应用到功能，自定义 `quality-checks` 报告，创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，您一直使用同一对话和分支。为了简化课程，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

您通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体在 PR 前综合了这些结果、需求审查和测试覆盖率。

新增测试应解决实际缺口；如果 QA 检查确认不需要添加测试，也可能是正确结果。授权合并前，请审查代码和证据；更改后应重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作方式。本研讨会中，您更新了指令、自定义了技能、创建了 QA 配置文件，并配置了 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令设定标准，技能描述可重复执行的任务，自定义智能体定义专用角色，而 MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择**模式和模型**。使用 **Plan** 在实现前考虑方案；使用 **Interactive** 参与范围明确的更改；使用 **Autopilot** 处理范围清晰的任务。常规编辑可选更快的模型，复杂工作则可选能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何实现，会显著影响输出。

## 继续探索

您已完成核心工作流。还可继续了解 Copilot CLI 的其他功能：

- 使用 `/review` 让代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得不同视角。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独的任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方式是持续实践。将 Copilot 用于生产代码、个人项目，或一直想构建却尚未着手的小应用。与团队分享经验，也向团队成员学习。最后，继续探索文档。

如果想从终端扩展 Tailspin Toys，可选学 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
