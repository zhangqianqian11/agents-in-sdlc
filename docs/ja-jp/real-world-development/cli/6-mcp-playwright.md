---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く以外にも、データや外部サービスとの連携、Copilot が利用する自動化ツールの追加が必要です。MCP server を使うと、Copilot CLI に組み込まれていないツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びます。
- Playwright MCP server がない場合は追加します。
- agent にブラウザーを操作させ、フィルタリング機能を確認します。

## シナリオ

unit test と end-to-end test は重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Web サイトを操作するためのツールを Copilot に渡し、変更の動作を確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI agent が外部ツールやサービスと通信するための仕組みです。MCP を使うと、agent は最新情報を取得したり、依頼に応じて操作したりできます。

ツールやリソースには MCP server を介してアクセスします。MCP server は、AI agent が利用できるツールとリソースのセットを提供します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プル リクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化を利用できます。

ほかにも多くの MCP server があり、GitHub は見つけやすさを高めるために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP server も、ほかの依存関係と同様に扱ってください。利用前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP server を追加する

すでに Copilot CLI で Playwright MCP server を設定している場合は、この手順をスキップして次に進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加画面を開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力して <kbd>Tab</kbd> を押します。
4. server type が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** の入力欄に次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) で MCP server を保存します。
7. <kbd>Esc</kbd> を押して MCP 画面を閉じます。

## Playwright を使って機能を確認する

Copilot が利用できる Playwright MCP server に、機能を検証するよう依頼します。

1. 次のプロンプトを送ります。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP server の使用を明示しなくても Copilot が判断します。ここでは使うツールを指定し、結果の一貫性を高めます。

2. Copilot がブラウザーで動作を確認する様子を見ます。
3. レポートを読み、期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP server を使い、実際のブラウザーで機能を確認しました。

- MCP と Copilot CLI での使い方を学びました。
- 必要に応じて Playwright MCP server を追加しました。
- agent にブラウザーを操作させてフィルタリング機能を確認しました。

次は、skill とブラウザー ツールをまとめた [QA custom agent を作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
