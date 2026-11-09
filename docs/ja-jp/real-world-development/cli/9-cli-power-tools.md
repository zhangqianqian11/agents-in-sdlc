---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキストの管理、モデルの選択、セッション共有、任意のクラウド委任に使える slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、多くの slash command が用意されています。これらの command を使うと、便利な機能、内部情報、追加設定にアクセスできます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、ほかの便利な command を確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で、Copilot が会話コンテキストを管理する方法を確認します。
- `/model` で、利用可能なモデルを確認します。
- `/share` でセッションを export または共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意の command を確認します。

## シナリオ

CLI の主なワークフローを終えたので、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、ほかの機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。slash command を使って、手動で確認や圧縮もできます。

1. Codespace に戻ります。Copilot CLI が起動していない場合は、リポジトリの root で起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在の token 使用量、システム instruction、tool、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合、大きな違いはないことがあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。圧縮のタイミングを選ぶには `/compact` を使います。無関係なタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、使用可能なモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

セッションを共有すると、チームで学びを共有し、AI tool の活用を改善できます。`/share` は、セッションを Markdown または HTML ファイルに export したり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. 共有する場合は、目的に合った出力先を選びます。たとえば `/share file` はローカルの Markdown file に export します。
3. export した内容を共有または公開する前に確認します。セッションの export には prompt、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## オプション: 作業の拡張や委任

ワークショップの主要な作業は完了しました。Copilot CLI には、より大きなタスク向けの command もあります。

- `/fleet` は独立したサブタスクを subagent に割り当て、並行して実行します。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は非同期に作業する Copilot cloud agent にタスクを送信し、プル リクエストを作成させることもできます。

これらの command は追加の worktree やリモート作業を作成するため、任意です。試す場合は、範囲を明確にした新しいタスクから始め、通常の手順で結果をレビューしてください。非同期 agent 作業を詳しく学ぶには、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定、セッション共有、内部状態の確認ができます。

- `/context` と `/compact` で会話コンテキストを確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションを export または共有する方法を学びました。
- 並行作業、worktree、cloud agent 委任の任意の command を確認しました。

ほかにも多くの slash command があります。次は、[学んだことを振り返り][next-lesson]、追加の学習方法を確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
