---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: custom instruction][previous-lesson]

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能追加などのタスクを自律的に実行できます。作業が終わると、人間がレビューできる draft PR を作成します。時間のかかる作業を任せることで、開発者はより大きな課題に集中できます。

このレッスンでは、次のことを確認します。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- 範囲が明確な Issue の重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には、最初のサイトを開発した契約者が残した技術的負債があります。特にドキュメントが不足しているため、まずアプリケーションの export 関数に TSDoc doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに同じカテゴリの別タイトルを表示し、UX の受け入れテストに使えるようにします。これは現在の blocker ですが、より優先度の高い Issue もあります。

どちらも後回しになりやすい作業です。Copilot cloud agent に非同期で任せることで、別の作業を続け、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent][copilot-agents] は人間の開発者と同様にバックグラウンドで作業します。[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法でタスクを依頼できます。割り当てると Copilot は進捗を記録する draft pull request を作成し、環境を準備して作業を開始します。実行中または完了後にセッションを確認できます。レビューの準備ができると、プル リクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える必要がなくなるわけではありません。一見簡単な操作にも複雑さがあります。[cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を明確にしてください。AI pair programmer として扱い、段階を踏んで学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリの custom instruction

前の演習では custom instruction とその効果を確認しました。まだ確認していない場合は、Copilot cloud agent に作業を割り当てる前に、このリポジトリにある instruction file を読み取り専用で確認してください。

GitHub web UI または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards**、特に TypeScript 規約と TSDoc doc comment の要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` に適用されることを確認します。

次の手順で `Code lacks documentation` Issue を cloud agent に割り当てます。PR に TSDoc doc comment、TypeScript 規約、ファイル コメント ヘッダーがあるかを確認してください。これらの規則は後の演習で PR をレビューするときにも使います。

## Copilot cloud agent 用の開発環境を準備する

コードの作成では、多くの場合、必要な tool と依存関係を揃えるための環境設定が必要です。Copilot にタスクを割り当てる場合も同様です。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` の[セットアップ workflow][setup-workflow]を作ると、作業前に実行する tool や依存関係を準備できます。この lab では、Node.js、プロジェクト依存関係、end-to-end test 用ブラウザー、単一 Astro app 用の SQLite database の migration と seed が事前に設定されています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブから setup step をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要最小限の permission を指定します。Copilot は操作用の token を別に受け取ります。
    permissions:
      # setup step で依存関係を入れるなど、リポジトリを clone する場合は contents: read が必要です。clone しない場合は step 完了後に Copilot が自動で clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Node.js を使うフロントエンド / app のセットアップ (app は Astro + Drizzle/libSQL)。
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # build と test でデータを使えるよう、ローカル SQLite database を migration して seed します。
      - name: Set up the database
        run: npm run db:setup
```

この workflow には、次の特徴があります。

- `copilot-setup-steps` という単一の job があり、Copilot が PR 作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` trigger を使うため、Copilot の実行を待たずにリポジトリの **Actions** タブから手動でテストできます。

## ドキュメントを追加する

ドキュメントは重要ですが、古くなっていたり不足していたりするプロジェクトは少なくありません。Copilot はドキュメント作成に適しているため、この作業を cloud agent に依頼します。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue**、続いて **Blank issue** を選択します。
4. **Title** に `Code lacks documentation` と入力します。
5. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. **Create** を選択します。
7. 右側の **Assign to Copilot** を選択し、割り当て画面で **Assign** を選択します。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

8. **Pull Requests** タブを開き、`[WIP]: Code lacks documentation` のようなタイトルの PR を探します。表示されない場合は少し待ってからブラウザーを更新します。
9. 数分後、Copilot が作成した todo list を確認します。

> [!NOTE]
> todo list が PR に表示されるまで数分かかる場合があります。Copilot は環境を準備し、プロジェクトを分析して、作業の方針を決めています。

10. todo list と実行予定のタスクを確認します。
11. PR の timeline を下にスクロールし、Copilot が作業を始めたことを確認します。
12. **View session** を選択します。

    ![Copilot session view](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えるように、ブラウザーの更新が必要な場合があります。

13. セッションを読み、Copilot がコードを探索し、状態を理解し、計画を考え、コードを作成する様子を確認します。

作業には数分かかります。cloud agent の利点は非同期にタスクを実行できることです。別の Issue も agent に割り当て、その間にアプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

Copilot cloud agent に別の作業を任せると、自分はほかの作業を進められます。関連ゲームの表示は短時間で実装できる場合でも、その時間を別の作業に使えるよう、Issue を割り当てます。

1. github.com のリポジトリに戻り、**Issues** タブを選択します。
2. **New issue**、続いて **Blank issue** を選択します。
3. **Title** に `Show related games on the game details page` と入力します。
4. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. **Create** を選択します。
6. 右側の **Assign to Copilot** を選択し、割り当て画面で **Assign** を選択します。

Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。cloud agent は software engineer と同じように非同期で動作するため、常に監視する必要はありません。完了後に結果を確認します。次は custom agent を作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] に Issue を割り当て、非同期に作業を依頼する方法を確認しました。技術的負債への対応、新機能の作成、framework の移行などを任せられます。

- コード生成環境をカスタマイズする方法を確認しました。
- 安全な操作が重要であることを確認しました。
- Issue の範囲を明確にする重要性を確認しました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次は [custom agent の作成と使用][next-lesson]に進みます。[cloud agent では MCP server][cloud-agent-mcp] も利用でき、前の演習で追加した custom instruction も適用されます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: Custom agent →][next-lesson] |
|:--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
