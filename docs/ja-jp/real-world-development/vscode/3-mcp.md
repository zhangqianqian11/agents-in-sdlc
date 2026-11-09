---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: Custom agent →][next-lesson]

agent mode でフィルタリング機能を作成しました。プル リクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに **Playwright MCP server** を接続し、Copilot に実際のブラウザーを操作させて機能をテストし、最後に branch を公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP server が Copilot を拡張する方法を学びます。
- workspace に Playwright MCP server を追加します。
- ブラウザーでフィルタリング機能を手動テストするよう Copilot に依頼します。
- branch を公開してフィルタリング作業の PR を作成します。

## Model Context Protocol (MCP) とは

agent mode は editor の外にある tool を使うと、さらに便利になります。Model Context Protocol (MCP) は Copilot が外部 tool や service と通信する標準的な方法です。

![agent mode、context、LLM、MCP server と VS Code extension が提供する tool の関係図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は AI agent が外部 tool や service と通信する方法を提供します。resource から最新情報を得たり、tool を使って代わりに操作したりできます。

MCP server は AI agent と外部 tool/service の間の bridge として動作し、既存 API や NPM package などの local tool との通信を管理します。各 server は、agent が使える tool と resource のセットを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub repository、Issue、pull request を管理する API にアクセスできます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright による browser automation を使い、ページ移動、フォーム入力、ボタン選択などを実行できます。

ほかにも多くの MCP server があり、GitHub は見つけやすさを高めるために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティに関して、MCP server はプロジェクト内のほかの依存関係と同様に扱ってください。利用前に source code を注意深く確認し、公開元を検証し、セキュリティへの影響を検討します。信頼できる MCP server のみを使い、機密 resource や操作へのアクセスを慎重に許可してください。

## MCP の構成を確認する

`.vscode/mcp.json` は、VS Code workspace で利用できる MCP server を設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` server がすでに設定されています。

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

`github` entry は project template に含まれているため、前の演習で Copilot は Issue backlog を読めました。[remote GitHub MCP server][remote-github-mcp-server] を使うため、local install は不要です。VS Code は HTTP で接続し、最初の tool 使用時に GitHub 認証を行います。

## Playwright MCP server を追加する

[Playwright MCP server][playwright-mcp-server] は、Copilot が操作できる browser を提供します。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` entry を追加します。

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

2. file を保存します。

remote の `github` と異なり、`playwright` は **local** server です。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面に browser window を表示せずに実行します。desktop がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys では end-to-end test に Playwright を使うため、browser は通常インストール済みです。Copilot が browser 不足を報告した場合は、`npx playwright install chromium` を実行して再試行します。

## Server を起動して信頼する

VS Code は MCP server を**必要に応じて**起動します。Copilot が最初に tool を必要としたときに server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` entry の上にある **Start** action を選択して起動し、設定を確認します。
2. server を信頼するか尋ねられたら、設定を確認して信頼を選択します。

手動起動は省略できます。その場合、次の手順で Copilot が tool を必要としたときに VS Code が起動し、信頼を確認します。

## フィルタリング機能をテストする

Playwright MCP server を使うと、Copilot はページを開き、移動し、filter を適用して結果を読み取れます。会話を離れずに機能を確認できます。

Playwright MCP server は screenshot ではなく、ページの [accessibility tree][playwright-mcp-server] を使います。button、link、list item など、構造化された label 付きの要素を agent が扱うため、簡単な機能確認とともに accessibility も確認できます。

server に接続し、app が実行中であることを確認したら、次の prompt で Copilot にフィルタリング機能を試させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server を介して browser を起動し、各手順を実行して結果を報告します。報告内容を Issue の受け入れ条件に照らし合わせます。問題があれば、PR を作成する前に質問するか、コード修正を依頼します。

> [!NOTE]
> テスト時には app が `http://localhost:4321` で実行されている必要があります。dev server を停止した場合は prompt を送る前に再起動します。Playwright MCP server の初回使用で browser の download が必要になる場合があります。不足が報告されたら `npx playwright install chromium` を実行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Branch を公開して pull request を作成する

1. Codespace で **Source Control** panel を開き、Copilot の変更をレビューします。
2. **+** icon を選択して変更を stage します。
3. **Sparkle** button を使って commit message を生成します。

   ![Copilot が変更した Source Control panel](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択し、branch を repository に push します。

## Pull request を作成する

github.com や GitHub CLI から PR を作成できますが、Copilot に Issue を探して関連付けた PR を作らせます。

1. Copilot Chat panel を開き、**New Chat** で新しい session を始めます。
2. agent dropdown で **Agent** を選び、GitHub tool を使えるようにします。
3. 次の prompt で PR 作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報収集や操作の実行に **Continue** が必要な場合は選択します。GitHub tool の初回使用では認証を求められることがあります。
5. Copilot が Issue を検索し、PR を作成したことを確認します。
6. Copilot が作った link から PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

- MCP と MCP server が Copilot を拡張する方法を学びました。
- workspace に Playwright MCP server を追加しました。
- PR 前に browser 上でフィルタリング機能をテストしました。
- branch を公開して PR を作成しました。

MCP server で機能をテストしました。次は、accessibility review のような作業を効率化するため、[custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
