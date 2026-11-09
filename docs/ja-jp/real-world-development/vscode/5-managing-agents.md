---
title: "演習 5 - Agent の監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: Custom agent][previous-lesson] |
|:--|

agent mode の GitHub Copilot は、codebase を調べ、変更を提案して file を編集し、command を実行します。VS Code の Copilot Chat では tool の呼び出しや file 編集をリアルタイムで確認できます。diff を確認して変更ごとに採用または却下し、追加 prompt で作業を調整できます。

この演習では、次のことを行います。

- accessibility agent の作業をリアルタイムに監視します。
- 提案された file 編集をレビューして採用します。
- session の途中で指示し、ハイ コントラストに加えてライト モードを追加します。

## シナリオ

[演習 4][previous-lesson]で選んだ accessibility custom agent は、ハイ コントラスト モードを実装しています。作業中、デザイン チームからライト モードも追加してほしいという依頼が来ました。新しい会話を始める代わりに、既存の session に追加指示を送ります。

## Copilot の進捗を監視する

Copilot Chat view には、agent が調査する file、実行する command、提案する編集が表示されます。編集は diff と **Keep** / **Undo** の操作とともに inline で表示されます。

1. 演習 4 の Copilot Chat view に戻ります。
2. chat をスクロールし、tool call、file 読み取り、編集、terminal command を確認します。
3. 提案された編集を確認します。
   - **Keep** を選ぶと変更を採用します。
   - **Undo** を選ぶと変更を元に戻し、agent に再試行させます。
4. chat 上部の **Files changed** で、この session の変更全体を確認できます。
5. agent がハイ コントラスト モードを実装し終えるまで待ちます。最後に変更の概要が表示されます。

> [!NOTE]
> terminal command の実行などで許可が必要な場合、agent は停止して **Allow** を表示します。内容を確認し、許可してよい command のみ承認してください。

## Agent にライト モードを追加するよう指示する

同じ会話を続けると、agent は先ほどの作業で変更した component、style、local storage の方式を把握したまま作業できます。

1. Copilot Chat の入力欄に次の prompt を送ります。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. agent が既存の変更を確認し、ライト モードを追加する様子を見ます。
3. 提案される編集をレビューし、各 diff で **Keep** または **Undo** を選択します。
4. 望まない方向に進んだ場合（例: toggle UI の意図しない refactor）は、入力欄の横にある **Stop** で停止し、追加の説明を送ります。

> [!TIP]
> session 中の指示変更は、最初からやり直すより速くできます。CSS を簡潔にする、設定 panel に統合する、keyboard shortcut を追加するなど、追加 prompt で修正や範囲の拡張を依頼します。

## 変更された file をレビューする

commit 前に agent が変更した内容を確認します。

1. VS Code で **Source Control** view を開きます。
2. 変更された file 一覧を確認します。Astro component、style、関連 test などが含まれます。
3. 変更 file を開いて diff を確認します。ARIA 属性、keyboard navigation、semantic HTML、local storage による保存などの accessibility pattern を確認します。
4. 問題がなければ Source Control panel から変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat agent mode で進行中の session を監視して方向を調整しました。各 file 編集をレビューし、変更を採用または却下し、追加 prompt で context を保ちながら作業範囲を広げました。

- agent の作業をリアルタイムで監視しました。
- 個々の file 編集を採用、却下しました。
- 追加 prompt で進行中の session を調整しました。

次は[アクセシビリティの変更を PR として公開し][next-lesson]、この lab で custom agent が作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode の使用][agent-mode]

---

| [← 前のレッスン: Custom agent][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
