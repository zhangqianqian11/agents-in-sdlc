---
title: "演習 4 - Agent の監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習で Copilot cloud agent に、ユーザー体験の改善や機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、進行状況を監視することも重要です。

GitHub.com の [agents page][agents-page] など、cloud agent に割り当てたタスクを管理する tool が用意されています。進行中の session を確認し、作業の方向を調整できます。

このレッスンでは、次のことを行います。

- agents page で cloud agent のタスクを監視します。
- 進行中の session に指示を送り、機能を追加します。

## シナリオ

チームはハイ コントラスト モードの追加に加えて、ライト モードも追加したいと考えました。サイトのスタイルと表示切り替えを変更しているため、agent にライト モードも実装するよう依頼します。

## Copilot cloud agent のタスクを確認する

1. [Copilot agents page](https://github.com/copilot/agents) を開きます。
2. main pane と左 pane のタスク一覧を確認します。割り当てたコード ドキュメントの追加、ゲーム変更 API の生成、Web サイトのハイ コントラスト モードなどが表示されます。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。リポジトリの checkout、作業環境の作成、MCP server のセットアップ、タスクを完了する手順などがあります。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や実装方法によって異なります。

4. 右側の PR pane で、監視対象の PR と変更ファイルも確認します。

## Cloud agent に指示を追加する

1. ハイ コントラスト モードを追加する session を選択します。タイトルは Copilot が付けた名前や作業状態によって異なります。

   ![Mission control に表示されるアクセシビリティ session](../../../_images/ex6-accessibility-session.png)

2. setup が完了して作業を始めるまで数分待ちます。次のようなメッセージが表示されたら作業中です。
3. **Steer active session while Copilot is working** dialog に次の prompt を入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Agents page の cloud agent task と、Steer active session while Copilot is working dialog](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押して prompt を送信します。
5. Copilot が指示を受け取り、作業に取り込むことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順を終えた後、新しい依頼を作業に追加します。完了まで数分かかることがあります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents page を、cloud agent task の監視と案内に使う方法を確認しました。進行中・完了済みの作業を追跡し、実行内容を確認し、session の範囲や指示を変更できます。

- agents page で cloud agent task を監視しました。
- 進行中の session に機能追加を依頼しました。

Copilot がアクセシビリティ機能を実装したら、次は [Copilot が作成した PR を反復して改善します][next-lesson]。

## リソース

- [Copilot agents page][agents-page]
- [Custom agent について][custom-agents]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
