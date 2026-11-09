---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、instruction、skill 更新、QA profile、テストを一緒にレビューして PR を作成し、Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、品質保証 (QA) profile、テストは、1 つのブランチに保存されています。まとめてレビューし、プル リクエスト (PR) を作成しましょう。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、instruction の更新、skill の更新、QA agent を複数の PR に分けます。ワークショップを簡潔にするため、フィルタリングと品質確認の作業を同じ会話とブランチで進め、この PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する仕組みを学びます。
- 機能全体と検証結果を確認します。
- フィルタリングの PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングの作業を通じ、Copilot で機能を計画、実装、検証しました。Tailspin Toys では、開発者がマージを許可する権限を保ったまま、PR に必要な残りの作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プル リクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗やレビュー コメントなど、PR を妨げている問題に対応し、必要に応じて rebase します。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge に任せる場合も、まず変更をレビューし、準備ができていると判断してから有効にしてください。

## Agent Merge で PR を管理する

1. Codespace に戻ります。
2. `/agent` を入力して agent dialog を開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視し、すべて成功したらマージします。
7. 「PR #14 was squash-merged successfully.」のような Copilot のメッセージを確認します。

> [!IMPORTANT]
> Agent Merge は必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は理由を確認し、リポジトリで許可されていればレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プル リクエストの処理など、開発プロセスの一部を自動化しました。

- Agent Merge とマージ ライフサイクルの自動化を学びました。
- 機能全体と検証結果を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、マージを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える[便利な Copilot CLI slash command を確認します][next-lesson]。

## リソース

- [Copilot CLI でプル リクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
