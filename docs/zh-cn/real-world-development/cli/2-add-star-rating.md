---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，现在可以开始对项目进行第一次更改了。游戏数据中已经包含星级评分，但主页上的游戏卡片尚未显示评分。您将让智能体显示评分、审查更改，并将它作为第一个拉取请求合并。

本课将学习如何：

- 在功能分支上启动专注于某项功能的 Copilot 对话。
- 让智能体对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详细信息页已显示该评分。但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身，您将让智能体在每张卡片上显示现有评分。这项小型且独立的更改非常适合第一次会话。

## 对话的组成

**对话**是您通过 Copilot CLI 处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，不会创建专用工作树。保存的对话可供日后继续，但文件和分支仍是磁盘上的普通 Git 状态。

对话中包含三类内容：您发送的提示和智能体的回复、智能体探索和编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

启动新的对话，开始实现功能：

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，请执行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到显示游戏详细信息的文件，并创建所需代码。您已使用 Copilot CLI 添加了一个新功能。

## 审查差异

所有 AI 生成的更改在合并前都需要审查，即使更改很小也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个更改的文件。
2. 确认游戏卡片在有评分时显示数值，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖上述两种情况。
4. 查看 Copilot 执行的检查结果，并要求它修复任何失败。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> Copilot 和其他生成式 AI 工具一样具有概率性，因此生成的代码可能与示例不同。请审查实际行为，而不是期待完全相同的实现。

## 检查更改

不能只阅读代码就假定它能正常工作。让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的界面。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

您已完成功能，现在可以创建拉取请求（PR），将新代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜，您已使用 GitHub Copilot CLI 发布了第一个更改！具体来说，您：

- 在功能分支上启动了专注于某项功能的 Copilot 对话。
- 让智能体对游戏卡片进行小幅更改。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，您将[从筛选 issue 开始，并使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
