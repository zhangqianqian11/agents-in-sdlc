---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支上。现在一并审查这些更改并创建拉取请求。您已自行合并星级评分拉取请求 (PR)；这次将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR。为简化研讨会，您将整个筛选和质量工作流保留在同一会话和分支中，并将所有更改放入这个 PR。

本课将学习如何：

- 了解 Agent Merge 及其如何自动化合并生命周期。
- 检查完整的功能 PR 和验证证据。
- 审查后才授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，您使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化 PR 剩余工作，同时让开发人员控制合并授权。

## 介绍 Agent Merge

**Agent Merge** 可在 GitHub Copilot app 中自动完成 PR 合并前的剩余工作。启用后，app 会话会读取 PR 并处理阻塞项，例如修复失败的持续集成 (CI) 检查、回应审查意见，以及必要时进行变基。GitHub 允许后，它会合并 PR。该功能在后台运行，app 重启后也会继续，并在 PR 合并后自动关闭。

此前由您亲自选择 **Merge pull request**。Agent Merge 可以代劳，但它编辑代码和合并仍需要您的明确授权。授予合并权限前，请审查允许执行的操作和已有工作。

## 使用 Agent Merge 管理 PR

所有代码都已创建并审查完毕，现在让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会更改为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮以启动流程。仅在下拉菜单中选择该选项不会启动流程。

Agent Merge 随即开始工作。它会：

- 创建包含标题和说明的拉取请求。
- 如果会话从 issue 启动，则在说明正文中引用相关 issue。
- 对目标分支进行变基或处理潜在合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控其他开发人员或 Copilot 代码审查的反馈，并进行更新以解决意见。
- 可选地在所有检查成功后自动合并 PR。

接下来，让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，先审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进智能体进度，并解决报告的阻塞项。继续前确认 PR 状态为 **Merged**；仅启用合并权限或智能体仍在运行，并不代表 PR 已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或缺失的权限。继续之前请解决这些阻塞项。

## 总结和后续步骤

您已自动化开发流程中的多个部分，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，您：

- 了解了 Agent Merge 及其如何自动化合并生命周期。
- 检查了完整的功能 PR 和验证证据。
- 审查后才授权 Agent Merge，并确认 PR 已合并。

接下来，通过[使用现有画布并创建分诊画布][next-lesson]，探索使用智能体检查、规划和可视化工作的更多方式。

## 资源

- [使用 GitHub Copilot app 管理 issue 和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
