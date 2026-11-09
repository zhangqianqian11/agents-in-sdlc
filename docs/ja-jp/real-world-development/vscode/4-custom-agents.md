---
title: "演習 4 - Custom agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## Custom agent とは

[GitHub Copilot の custom agent][custom-agents-concept] は、開発 workflow の特定のタスクや分野に合わせた専門 AI assistant です。リポジトリの `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown file で定義します。YAML frontmatter には少なくとも `name` と `description` を指定し、その後に agent の役割、専門知識、instruction を記載します。チームの知識を再利用可能な agent にまとめると、WCAG に沿う accessibility agent、secure coding を適用する security agent、test pattern を維持する testing agent などを作成できます。

### Custom agent と agent skill の比較

custom agent と [agent skill][agent-skills-concept] はどちらも主に Markdown file で定義し、AI の作業方法を指示します。分かりやすく言うと、custom agent は作業者、skill は tool です。

custom agent は独自の context window を持ち、skill やほかの agent の作業を調整します。この lab の accessibility custom agent は、ガイドラインに沿ってサイトをレビュー、更新します。その作業の一部として、PR workflow や test 実行用の skill を使うこともできます。

> [!NOTE]
> custom agent の作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰でも利用できるクラウドファンディング platform を目指しています。現在のダーク テーマは文字と背景のコントラストが不足し、読みにくいという意見が寄せられました。デザイン チームは、切り替え可能なハイ コントラスト モードを求めています。accessibility 用 custom agent を使って機能を実装します。

この演習では、次のことを行います。

- 既存の accessibility custom agent を確認します。
- Copilot Chat で accessibility agent を使ってハイ コントラスト モードを実装します。

## Accessibility custom agent を確認する

1. Codespace に戻り、terminal を開いて `main` から accessibility 作業用の新しい branch を作ります。Exercise 3 のフィルタリング PR と分けて作業します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開き、`name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> custom agent には `name` と `description` の frontmatter が必要です。

3. アクセシブルなサイト向けの責務、ベスト プラクティス、HTML/CSS/JavaScript の例、よくある問題を確認します。

## Copilot Chat で custom agent を使う

VS Code は `.github/agents` に定義された custom agent を、Copilot Chat view 下部の agent dropdown に表示します。agent を選ぶと、その agent の instruction と tool を使う session になります。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選び、新しい会話を始めます。

1. Chat view の agent dropdown で **Agent** を選択します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

2. chat view 下部の agent dropdown（既定では **default**）を開きます。
3. 使用可能な agent から **Accessibility agent** を選択します。
4. 次の prompt を送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat が codebase を調べ、変更を提案して project file を編集します。編集は file path と diff とともに Chat に表示されます。
6. **Files changed** の表示で、変更された file を確認します。

> [!NOTE]
> 実際に repository を変更するため、数分かかることがあります。Astro component、CSS、関連 test などが編集されます。

次の演習では作業中の変更を確認し、追加の指示を送ります。

## まとめと次のステップ

[GitHub Copilot custom agent][custom-agents] を使い、特定のタスクや分野に合わせた専門 assistant を確認しました。

- custom agent の定義を確認しました。
- Copilot Chat agent mode で custom agent を使いました。

次は [agent の作業を監視して方向を調整します][next-lesson]。変更を確認し、同じ session にライト モードの切り替えを追加します。

## リソース

- [Custom agent について][custom-agents]
- [IDE で custom agent を作成する][creating-custom-agents-ide]
- [VS Code の custom agent][custom-agents-vscode]
- [Custom agent の構成][custom-agents-config]
- [awesome-copilot の custom agent][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: Agent の監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
