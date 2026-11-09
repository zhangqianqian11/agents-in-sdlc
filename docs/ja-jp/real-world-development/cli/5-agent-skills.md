---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズしてフィルタリングを検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、テスト、lint、継続的インテグレーション (CI) などの作業があります。このような繰り返しの作業には **agent skill** が適しています。skill は、操作の実行方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果のレポート形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、unit test、lint、型チェックに `quality-checks` skill を使います。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## Instruction、script、リソース

Agent skill は、再利用可能なタスク指示、実行可能な script、補助リソースをまとめたものです。skill のフォルダーには `SKILL.md` という Markdown ファイルがあります。frontmatter の name と description、概要、呼び出すタイミングのガイダンスを記載します。必要に応じて、script やほかのリソースを含むサブフォルダーも追加できます。

> [!NOTE]
> skill に追加ファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトの既存コマンドを使うため、`SKILL.md` だけで構成されています。

skill は、チームで共有するプロジェクトの `.github/skills` フォルダー、またはユーザー用の `~/.copilot/skills` フォルダーに置けます。

## skill を確認する

1. Codespace に戻り、Codespaces editor で `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は Copilot が skill を呼び出すタイミングを判断するのに役立ちます。
3. テストと lint の手順を確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新後の skill を実行する

Copilot CLI は会話を再起動せずに、編集後の skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と同じプロンプトを送ります。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. レポートを読み、最初の結果と比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使用しました。

- unit test、lint、型チェック用の `quality-checks` skill を確認しました。
- 結果のレポート形式をカスタマイズしました。
- skill を再読み込みして実行しました。

変更は機能 PR に含めます。次は [Playwright MCP server を使って Copilot がサイトを操作できるようにします][next-lesson]。

## skill の例

以下のコミュニティ例は参考資料です。追加の作業はありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図と export script: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
