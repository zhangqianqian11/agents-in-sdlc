---
title: "レッスン 7 - QA agent を作成して使う"
description: "Issue の要件、quality-checks skill、Playwright MCP を組み合わせる QA custom agent を作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill で自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。ここでは、これらの機能を QA の手順を定義した custom agent にまとめます。

このレッスンでは、次のことを行います。

- custom agent が instruction、skill、MCP tool と連携する方法を確認します。
- 再利用可能な品質保証 profile を作成して確認します。
- QA agent を選択し、フィルタリングの Issue に対する結果を確認します。

## シナリオ

Tailspin Toys は、プル リクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫して確認したいと考えています。custom agent を使うと、QA の手順を実行し、再利用可能なレポートを作成できます。

## Custom agent とは

custom agent は、Markdown profile で定義する Copilot の専門的な役割です。profile には、目的、instruction、使用可能な tool を記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリ instruction はチーム標準を説明し、`quality-checks` skill は繰り返し実行するチェックをまとめ、Playwright MCP はブラウザー tool を提供します。QA profile は、これらを使って要件を評価し、結果を報告する方法を指示します。

## QA profile を作成する

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. default agent に次の custom agent を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Profile を確認する

新しい agent を使う前に、QA の手順と制約が意図どおりに記述されていることを確認します。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、表示名として役立ちます。
3. 要件から始め、リポジトリ instruction に従い、`quality-checks` skill を実行して Playwright MCP を使うことを確認します。
4. 根拠を含む結果を報告し、実装コードを変更する前に確認を求め、commit や PR の作成を行わないことを確認します。
5. 抜けている責務や制約があれば、続ける前に default agent に修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクト agent を読み込みます。profile を作成した後、同じフィルタリングの会話を再開し、QA を選択します。

1. 次の prompt で agent を有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりの agent が選択肢に表示されない場合がありますが、上記コマンドで有効にできます。

2. 次の prompt で機能をレビューします。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA agent の作業が終わったら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに追加し、その結果を確認しました。

- custom agent と instruction、skill、MCP tool の連携を確認しました。
- 再利用可能な QA profile を作成して確認しました。
- QA agent を選択し、Issue に対する結果を確認しました。

実装、skill の更新、QA profile、テスト、検証結果が揃いました。次は[機能 PR をまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用 custom agent を作成する][create-agents]
- [Custom agent の構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
