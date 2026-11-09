---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づくフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初のレッスンでは、小さな機能を追加しました。より大きな変更には、さらに体系的なプロセスが必要です。GitHub Copilot CLI は既存の開発フローに沿って作業できます。このレッスンでは、Issue から機能を実装し、コードと動作を検証して、プロジェクトにマージするまでの agent 主導の開発を始めます。

> [!NOTE]
> この機能の作業では、同じ会話とブランチを使います。通常ならファイルの種類ごとにブランチや PR を分けますが、ここでは主な概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この後、リポジトリの instruction を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して QA agent を作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えているため、訪問者がゲームをカテゴリや発行元で絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に決めておく点があります。Plan モードで要件を決めてから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング agent を開発に取り入れても、基本的な開発プロセスは変わりません。一般的な流れは次のとおりです。

1. 必要な作業を記載した Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プル リクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、マージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、ほとんどの場合はこの流れを基本にしています。

この標準的な進め方に従えば、AI が生成したコードも要件を満たし、手作業で書いたコードと同じレビュー プロセスを通せます。

## 会話モード

**会話モード**は、agent にどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: agent と一緒に作業します。agent は変更を提案し、次に進む前に入力を待ちます。
- **Plan**: agent が最初に計画を作成します。プロジェクト ファイルは編集できません。
- **Autopilot**: agent が自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してフィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** という Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7`（または同様の番号）で始まることに注目してください。`#` を使うと GitHub の Issue や PR をコンテキストに含められます。

## フィルタリング機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に依頼する前に、方針を決められます。複雑な作業では、時間をかけて計画するのが効果的です。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 先ほど追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調べ、実装方法を検討して計画を作ります。

3. フィルタリングの動作について質問された場合は、希望に応じて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読みます。
6. 決定した内容と合わない箇所があれば、修正を依頼します。

## Autopilot を承認する

計画のレビューが終わったら、実装に進みます。Autopilot は、タスクが完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)**（またはインストール済みバージョンで同様のラベルの選択肢）を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まることがあります。先に計画をレビューしてください。依存関係の不足やポートの競合が報告された場合は、チェック完了と判断する前に環境の問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様にマージ前にレビューします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を Issue と計画時の決定に照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

Codespaces の転送ポートからサイトを開き、実際に動かしてみます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. カテゴリ、発行元、および計画で決めた組み合わせによるフィルタリングを試します。
4. リセットと結果が空の場合の動作が、Issue と計画時の決定に合っていることを確認します。
5. Codespace に戻り、開発サーバーの停止を Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を実装し、レビューしました。

- GitHub Issue から Copilot の会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[custom instruction を使って][next-lesson]、コードが文書化されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
