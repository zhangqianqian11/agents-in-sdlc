---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款基于 GitHub Copilot CLI 构建的智能体驱动开发桌面应用，并与 GitHub 原生集成，因此存储库、分支和 CI 管道开箱即用。它面向需要同时指挥多个智能体的工作流，每个智能体都在独立工作区中运行；您可以将重复任务交给智能体，而不必亲自处理所有工作。安装 Node.js 并准备好项目副本后，接下来安装 app 并连接存储库。

本课将学习如何：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到 app。
- 熟悉工作区，包括模板预置的待办事项。
- 通过快速聊天了解 app 本身。

## 场景

团队正采用 AI 智能体处理不断增长的待办事项。Copilot app 提供统一工作位置，供您领取 issue、运行智能体、审查更改并合并拉取请求。本课会完成安装、连接项目，并让您熟悉如何围绕项目发起对话。

> [!NOTE]
> 需要符合条件的 Copilot 方案：Copilot Free、Copilot Student 或任何付费方案（Pro、Pro+、Business 或 Enterprise）。使用 Copilot Business 或 Copilot Enterprise 时，管理员必须启用 **GitHub Copilot app** 策略。

## 安装并配置 GitHub Copilot app

第一步是安装 GitHub Copilot app。该应用适用于 Windows、macOS 和 Linux。接下来安装 app、完成身份验证并添加 Tailspin Toys 存储库。

1. 在浏览器中打开 [GitHub Copilot app 产品页][download-app]。
2. 下载适用于当前平台的 app，并按照产品页上的说明进行安装。
3. 安装完成后打开 app。
4. 选择 **Sign in to GitHub** 并按照提示完成身份验证。如果使用具有数据驻留的 GitHub Enterprise Cloud，请在出现提示时选择 **Use GitHub Enterprise** 并输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果入门流程提供存储库选择，请选择从模板创建的 Tailspin Toys 存储库。也可以跳过存储库选择，在完成入门流程后再添加项目。
6. 按提示选择主题，然后完成入门流程。
7. 在侧边栏的 **Sessions** 下查看存储库。如果已列出，则无需再次添加。
8. 如果尚未连接存储库，请选择 **Sessions** 旁的 **+** 创建新会话，再选择 **Add GitHub repository**。输入准备工作课程中记下的所有者和存储库名称。
9. 确认存储库出现在侧边栏中。当前布局会将 `tailspin-toys` 等存储库文件夹归在 **Sessions** 下，并在每个存储库下显示独立会话。

## 熟悉工作区

连接项目后，花些时间熟悉工作区。侧边栏将功能组织在以下区域：

- **New**：启动与 Copilot 的新聊天会话。
- **Pull requests**：通过 app 的 GitHub 原生集成查看拉取请求。在这里可以浏览和筛选拉取请求、查看 CI 状态并审查 PR，无需离开 app。
- **Issues**：以同样的方式查看 issue。您可以浏览和筛选 issue，并从 issue 启动会话。
- **Automations**：保存按计划或按需运行的智能体任务，适合管理待办事项、定期项目维护或其他重复工作。总结课程会将其作为后续学习方向，而不是额外的研讨会练习。
- **Customize**：通过 MCP 服务器、插件、技能和其他组件为 Copilot app 添加功能。您会在此配置 Playwright MCP。
- **Chats**：用于提问和头脑风暴的轻量对话，不需要单独创建分支或工作区。开始第一个聊天后，此区域才会出现。如果尚未显示，请选择 **Sessions** 旁的 **+** 开始第一次聊天。
- **Sessions**：存放存储库文件夹及其智能体会话。选择 `tailspin-toys` 等存储库名称可打开存储库视图；选择存储库下的某个会话可返回对应对话。每个会话都在独立工作区中运行，因此可并行运行多个会话而不会发生更改冲突。下一课会启动第一个会话。

完成研讨会的过程中，您会逐步了解工作区。

> [!TIP]
> 遇到疑问时，可以向 Copilot 提问。如果不确定如何操作或某项功能是否可用，Copilot 可以提供指导。

### 查找模板预置的待办事项

几乎每个项目都有待办事项，Tailspin Toys 也不例外。现在查看您创建模板时预置的待办事项。

1. 在侧边栏 **Sessions** 下选择 Tailspin Toys 存储库文件夹，而不是其下的某个会话。然后在存储库顶部导航中选择 **Issues**。
2. 根据标题查找以下 issue，不要假设它们的编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择一个 issue 查看详情。每个 issue 都可以作为智能体会话的起点。完成第一次小更改后，您会从筛选 issue 启动会话。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示已添加到 Copilot app 的存储库中的项目。若要查看其他存储库的工作项，请将这些存储库添加到 app。

## 尝试快速聊天

熟悉 app 的一种好方法是用它了解*应用本身*。**快速聊天**允许您提问或头脑风暴，而无需创建分支或工作树，非常适合不需要会话的一次性问题。

1. 在侧边栏中选择 **Chats** 旁的 **+**，打开新聊天。如果尚未显示 **Chats**，请先选择 **Sessions** 旁的 **+** 开始第一次聊天；之后便会出现 **Chats**。
2. 询问 app 的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。每个会话都在独立的 Git 工作树中运行，因此可以并行运行多个智能体而不发生更改冲突。您可以随时继续对话或开始新聊天。

## 总结和后续步骤

恭喜，您已安装 GitHub Copilot app、连接项目并探索工作区。您学习了如何：

- 安装 app 并登录 GitHub。
- 从 GitHub 存储库添加项目。
- 熟悉工作区，并在 **Issues** 中查找模板预置的待办事项。
- 使用快速聊天提出一次性问题。

接下来，您将[启动第一个智能体会话][next-lesson]，使用它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
