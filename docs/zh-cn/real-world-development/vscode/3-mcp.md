---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

您刚刚使用智能体模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能确实正常。无需亲自逐项操作应用，而是连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当智能体模式可以访问编辑器以外的资源时，它的能力会大幅增强。Copilot 使用 Model Context Protocol (MCP) 与这些资源交互；这是一种让智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作原理示意图，显示它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供与外部工具和服务通信的方式，使它们能够实时获取最新信息（使用资源）并代表您执行操作（使用工具）。

智能体通过 MCP 服务器访问工具和资源。MCP 服务器是智能体与外部工具和服务之间的桥梁，负责管理通信；外部工具可以是现有 API，也可以是 NPM 包等本地工具。每个 MCP 服务器都提供一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，让智能体可以创建存储库、更新现有存储库以及管理 issue 和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让智能体可以浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 对待 MCP 服务器应与对待项目中的其他依赖项一样谨慎。使用前请仔细审查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 文件中应已为您配置 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板已包含 `github` 条目，因此 Copilot 能在上一课中读取 issue 待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，首次使用其工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供受控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 参数会让 Playwright 在没有可见窗口的情况下运行浏览器，这适用于没有桌面环境的 codespace。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问您是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过，VS Code 会在下一步中 Copilot 首次需要工具时启动服务器，并询问是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可让 Copilot 操作真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后向您总结观察情况，无需您离开对话亲自操作应用。

Playwright MCP 服务器通过页面的[无障碍树][playwright-mcp-server]工作，而不是依赖截图。因此智能体会分析带有标签的结构化元素（按钮、链接和列表项），方式与辅助技术类似；快速功能检查也能兼顾基本的无障碍检查。

服务器已连接且应用正在运行时，要求 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、执行各项操作并报告结果。请根据 issue 中的验收标准审查报告；如果发现问题，可继续提问或让 Copilot 修复代码，再创建拉取请求。

> [!NOTE]
> 测试时应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，可以创建拉取请求（PR）供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板并审查 Copilot 所做更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建拉取请求。也可以让 Copilot 查找相关 issue 并创建关联 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 执行获取信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求验证 GitHub 身份；请按照提示操作。
5. 注意 Copilot 如何搜索 issue、找到相关项并创建 PR。
6. 选择 Copilot 生成的链接审查 PR，但**暂时不要合并**。

## 总结和后续步骤

恭喜，本练习中您：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用它在浏览器中手动测试筛选功能。
- 发布了分支并为筛选工作创建了拉取请求。

您使用 MCP 服务器测试了功能，但 MCP 还有其他用途。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专注任务。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
