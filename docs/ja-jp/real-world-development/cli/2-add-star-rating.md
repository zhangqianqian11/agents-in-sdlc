---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲーム カードを変更し、Codespaces のブラウザーで確認して、最初のプル リクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えましょう。変更は小さくします。ゲームのデータには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。agent に評価を表示させ、変更をレビューして、最初のプル リクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を始めます。
- 小さな変更を agent に依頼します。
- `/diff` で変更をレビューします。
- アプリを起動し、転送されたブラウザーで変更を確認します。
- 最初のプル リクエスト (PR) を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホーム ページのカードにはタイトル、カテゴリ、発行元、説明だけが表示されます。最初の練習として、既存の評価を各カードに表示します。最初のセッションに適した、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI とタスクに取り組む場所です。Copilot app と違い、通常の CLI 会話では専用の worktree を作らず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存した会話を後から再開できます。ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトと agent の応答、ファイルを調査・編集する agent のツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot CLI が起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. slash command `/new` を入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトを送信します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot がプロジェクトを調べ、ゲーム詳細の表示に使われているファイルを見つけ、必要なコードを作成します。これで Copilot CLI を使った機能追加ができました。

## 差分をレビューする

AI が生成した変更は、小さなものでもマージ前にレビューします。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` がゲーム カードに表示されることを確認します。
3. どちらの状態もテストされていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、生成コードは毎回異なることがあります。実装の完全一致を期待せず、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Codespaces で転送されたブラウザーからサイトを確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲーム カードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプル リクエストを作成してマージする

1. default agent に変更の commit を依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存の commit を push し、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースしました。

- 機能ブランチで Copilot の会話を始めました。
- agent にゲーム カードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーで星評価を確認しました。
- 最初のプル リクエストを作成してマージしました。

次は、より大きな機能を実装するため、[フィルタリングの Issue から Plan と Autopilot モードを使います][next-lesson]。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
