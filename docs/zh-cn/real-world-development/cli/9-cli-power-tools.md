---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "了解用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可用于访问高级功能、查看内部信息和配置选项。您已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在继续了解其他实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索并行工作、工作树和委派到云智能体等可选命令。

## 场景

核心 CLI 工作流已完成。现在了解更多功能，包括管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；您也可以使用斜杠命令检查或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请在存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统指令、工具、消息和剩余空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口即将填满时自动压缩上下文。想自行决定压缩时机时，可使用 `/compact`。切换到无关任务并希望开始新对话时，则使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也有各自偏好。Copilot CLI 允许您查看并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员共享经验和学习成果，有助于大家更好地使用 AI 工具。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前先进行审查，因为会话导出可能包含提示、回复和项目详细信息。

发布链接或 gist 是可选操作。不要发布团队无意共享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会至此结束。Copilot CLI 还为大型任务提供了以下命令：

- `/fleet` 可将独立子任务交给多个子智能体并行执行。
- `/worktree` 可为单独的任务创建隔离 Git 工作树。
- `/delegate` 可将任务发送给异步工作的 Copilot 云智能体；它可能会创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或远程工作。尝试前先创建范围明确的新任务，并通过正常工作流审查结果。若要进一步了解异步智能体，请继续学习[云智能体课程][cloud-workshop]。

## 总结和后续步骤

Copilot CLI 斜杠命令可用于配置工具、共享会话并查看内部状态。本课中，您：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索了并行工作、工作树和委派到云智能体等可选命令。

Copilot CLI 还有更多命令和功能可供探索。现在通过[回顾所学内容][next-lesson]并查看后续学习资源，为本次学习收尾。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
