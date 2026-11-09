---
title: "第 2 课 - 添加星级评分：快速改进"
description: "在 GitHub Copilot app 中启动第一个智能体会话，对游戏卡片进行小幅更改，并创建第一个拉取请求进行合并。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

浏览工作区并尝试快速聊天后，现在可以启动**智能体会话**，对项目进行第一次更改。此次改动很小：游戏数据中已经有星级评分，但主页的游戏卡片尚未显示评分。您将让智能体显示评分、审查更改，并通过第一个拉取请求合并更改。

本课将学习如何：

- 启动智能体会话并了解会话结构。
- 让智能体对项目进行小幅且范围明确的更改。
- 在工作区差异视图中审查更改。
- 在本地运行应用并在浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详细信息页已显示该评分。但主页的游戏卡片只显示标题、类别、发行商和说明。作为热身，您将让智能体在每张卡片上显示现有评分。这项小型且独立的更改非常适合第一个会话。

## 会话的组成

**会话**是与智能体的对话，在独立工作区中运行。每个会话都有专用的 **git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复 bug，而不会造成更改冲突。会话按存储库分组显示在侧边栏中，选择任一会话即可切换。

会话中包含三类内容：与智能体的**对话**、智能体探索和编辑文件时的**工具活动**，以及附带差异的**已更改文件**列表。

## 启动会话并请求更改

启动新会话，开始探索项目并实现功能。在[应用设置][prior-lesson]期间，您已从 GitHub 存储库添加项目。现在为该存储库创建新会话并请求更改。

1. 返回（或打开）GitHub Copilot app。
2. 选择侧边栏顶部的 **+**，启动会话。
3. 为存储库选择 `tailspin-toys`。
4. 在提示框下方选择 **new working tree** 和 **Interactive** 模式。使用以下提示请求更改：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树，即项目的隔离副本。然后探索项目，找到需要更新的文件，并创建实现功能所需的代码。您已使用 Copilot app 添加了新功能。

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使更改很小也不例外。现在直接在 Copilot app 中检查更改。

1. 在 app 右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看更改的文件和差异。

    ![GitHub Copilot app 顶部工具栏，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 注意 `GameCard.astro` 中添加的代码，该文件用于显示游戏详细信息。代码应与以下示例类似：有评分时显示评分，`starRating` 为 `null` 时显示 `No rating yet`：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot 与所有生成式 AI 工具一样具有概率性，因此实际代码可能与示例不同，但应大致相似。

## 检查更改

打开浏览器前，先审查智能体的自动化检查结果。确认测试覆盖数值类型的 `starRating` 和 `null` 回退状态。缺少先决条件或跳过的检查不算通过；批准任何安装请求前先审查。

不能只阅读代码就假定它能正常工作。让 Copilot 启动网站，以便在浏览器画布中检查更新后的 UI。

> [!TIP]
> 画布是 Copilot app 内的交互式小组件。稍后您会探索自定义画布，甚至创建自己的画布；本课先使用内置浏览器画布。

1. 使用以下提示让 Copilot 启动应用并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 稍等片刻，应用将启动，Copilot app 内会打开浏览器窗口。
3. 确认已评分的游戏卡片显示满分为五分的评分。
4. 完成后，使用以下提示让 Copilot 停止为此会话启动的开发服务器并关闭浏览器画布：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 创建并合并第一个拉取请求

您已完成功能。现在创建拉取请求（PR），将新代码合并到项目中。

1. 选择右上角的 **Create PR**。
2. 如果系统提示，请选择 **Sign in with your browser**，并按照提示完成身份验证。
3. 如果 Copilot 说明存储库策略要求先关联 issue 才能创建 PR，请确认让它遵循此流程。
4. Copilot 将开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查面板中打开 PR 并查看内容。
6. 准备就绪后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并 PR。

## 总结和后续步骤

恭喜，您已使用 GitHub Copilot app 发布第一个更改。本课中，您：

- 启动了智能体会话，并了解了会话结构。
- 让智能体对游戏卡片进行小幅且范围明确的更改。
- 在工作区差异视图中审查了更改。
- 在本地运行应用，并在浏览器中确认了星级评分。
- 打开了 PR 1、审查了检查结果并明确执行合并。

接下来，您将[从筛选 issue 开始，并使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [使用 GitHub Copilot app 管理 issue 和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#安装并配置-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
