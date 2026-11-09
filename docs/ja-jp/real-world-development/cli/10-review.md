---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプル リクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で一連の作業を進めながら、GitHub Copilot CLI を使いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、フィルタリングの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで確認し、最初のプル リクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- custom instruction で agent を導き、既存の `quality-checks` skill をカスタマイズして unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを確認しました。
- 要件、テスト範囲、skill の結果、ブラウザーでの証拠を評価する品質保証 (QA) custom agent を作成して選択しました。
- フィルタリングの変更全体をレビューし、PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使える slash command を確認しました。

## リリースした内容

このワークショップには、更新した `main` から作成したブランチで、2 つの PR を作成しました。

1. **星評価**: ゲーム カードに既存の `starRating` と未評価状態を表示します。
2. **フィルタリングと品質確認**: フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA profile と関連テストを追加します。

フィルタリングの計画から PR 作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作でコードを確認しました。`quality-checks` skill は unit test、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA は要件とテスト範囲のレビューをこれらの結果に加えて、PR の前にまとめました。

追加テストは実際の不足を補うものであるべきです。QA 実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証結果を確認し、変更後は関連する検証結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストと tool が、作業の結果に影響します。このワークショップでは instruction を更新し、skill をカスタマイズし、QA profile を作成し、MCP server を設定しました。これらを会話間で再利用し、チームのニーズに合わせて調整できます。instruction は標準を定め、skill は繰り返し可能なタスクを説明し、custom agent は専門の役割を定義し、MCP server は外部 tool に接続します。agent の要約だけでなく、実際の変更と tool の結果を確認してください。

タスクに合わせて **mode と model** を選びます。実装前の検討には **Plan**、範囲を絞った変更には **Interactive**、明確なタスクには **Autopilot** を使います。定型的な編集には高速な model、複雑な作業には高性能な model を選びます。

ツールや環境と同様に、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確に伝えると、結果が変わります。

## さらに確認する

Copilot CLI のほかの機能も試せます。

- `/review` で code review agent に変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、継続して使うことが大切です。製品コード、趣味のコード、作りたいと思っていた小さなアプリなどで活用してください。チームと学びを共有し、メンバーからも学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張するには、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境を試すには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプル リクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
