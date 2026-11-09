---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都保存在同一分支上。现在一起审查这些更改并创建拉取请求。本次不再手动合并星级评分 PR，而是让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本研讨会，您在同一对话和分支中完成了筛选和质量工作，并将这些更改一起放入此 PR。

本课将学习如何：

- 了解 Agent Merge 以及它如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 完成审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，您使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 希望自动化 PR 剩余工作，同时让开发者保留合并授权权。

## 介绍 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复失败的持续集成（CI）检查、回应审查意见，以及必要时进行变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

此前由您亲自选择 **Merge pull request**。Agent Merge 可以代劳，但启用前仍应先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码完成后，一起审查更改、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 将监控 PR 的持续集成流程。检查全部成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必要的审批、分支保护、合并队列、存储库设置或权限限制。如果合并受阻，请阅读报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

您已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，您：

- 了解了 Agent Merge 以及它如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，您将[探索 Copilot CLI 中更多实用的斜杠命令][next-lesson]，管理上下文和模型、共享会话，并了解可选的云端委派功能。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
