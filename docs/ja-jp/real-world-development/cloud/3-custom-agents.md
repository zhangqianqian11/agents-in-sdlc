---
title: "演習 3 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

[GitHub Copilot の custom agent][custom-agents-concept] は、開発ワークフローの特定のタスクや分野に合わせて作成する、専門的な AI assistant です。リポジトリの `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。YAML frontmatter には少なくとも `name` と `description` を指定し、その後に agent の役割、専門知識、instruction を記載します。チームの知識を再利用可能な agent にまとめると、アクセシビリティ、セキュリティ、テストなどの分野に応じて作業を導けます。

### Custom agent と agent skill の違い

custom agent と [agent skill][agent-skills-concept] はどちらも主に Markdown file で定義され、AI の作業方法を指示します。custom agent は作業者、skill は tool と考えると区別しやすくなります。

custom agent は独自のコンテキスト ウィンドウを持ち、skill やほかの agent を調整して作業できます。この演習では、アクセシビリティ custom agent がガイドラインに沿ってサイトをレビュー、更新します。作業中に pull request workflow skill やテスト管理 skill を呼び出すこともできます。

> [!NOTE]
> custom agent の作成に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次のことを確認します。

- custom agent の定義方法。
- custom agent にタスクを割り当てる方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング サイトを利用できるようにしたいと考えています。ユーザーから、文字と背景のコントラストが不足していてダーク テーマが読みづらいという意見が寄せられました。デザイン チームは、切り替え可能なハイ コントラスト モードを求めています。

アクセシビリティは重要なため、custom agent を使って機能をすばやく実装します。

## アクセシビリティ custom agent を確認する

アクセシビリティ用の custom agent はすでに用意されています。内容を確認し、Copilot を導く方法を理解します。

1. Codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> custom agent には `name` と `description` の frontmatter が必要です。

3. 続くセクションを読み、アクセシブルな Web サイト用コードの責務、ベスト プラクティス、HTML/CSS/JavaScript の例、よくある問題を確認します。

## Issue を作成して割り当てる

Mission control では agent の作業を割り当て、監視し、進行中の session に追加指示を送れます。まずハイ コントラスト モードの実装を Copilot に任せます。

1. リポジトリを開き、**Issues** タブを選択します。
2. **New issue**、続いて **Blank issue** を選択します。
3. **Title** に `Add high contrast mode to website` と入力します。
4. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. **Create** を選択し、右側の **Assign to Copilot** を選択します。
6. 一覧から **Accessibility agent** を選び、**Assign** を選択します。

   ![Accessibility custom agent を指定して cloud agent に割り当てる画面](../../../_images/ex5-select-custom-agent.png)

Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

[GitHub Copilot の custom agent][custom-agents] を確認しました。専門的なタスクや分野に合わせて agent を作成し、チームの知識や標準を再利用可能な形にまとめられます。

- custom agent の定義方法を確認しました。
- agent にタスクを割り当てました。

次は [mission control から agent session を監視して方向を調整します][next-lesson]。

## リソース

- [Custom agent について][custom-agents]
- [組織で custom agent を使う準備][org-custom-agents]
- [Enterprise で custom agent を使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: Agent の監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
