---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリ更新でも、複数の file の編集やテスト実行が必要になることがあります。開発者は必要な file を見つけて変更し、test を実行し、問題を修正し、漏れていた file を探して、再び更新します。このような作業に Copilot Agent Mode が役立ちます。

Copilot Agent Mode は IDE 内でより自律的に作業します。既存の project 構造を調べ、必要な file を更新し、test などの task を実行し、発見した問題を修正します。ここでは、Agent Mode でサイトに機能を追加します。

> [!NOTE]
> agent mode と cloud agent は名前が似ていますが、用途が異なります。agent mode は IDE 内で動作し、短い feedback loop と対話を実現します。cloud agent はチーム メンバーのように非同期で動き、Issue と PR を通してやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode で project を調べ、関連 file を特定して一連の変更を行う方法。
- UI とデータ レイヤーにまたがる新機能を実装する方法。
- codebase にマージする前に生成された変更と test をレビューする方法。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリと発行元で絞り込めるようにします。前の演習で publishers helper を追加しました。残りのデータ レイヤー、UI、test の作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを実行する

変更前に、Tailspin Toys サイトの現在の動作を確認します。これは board game のクラウドファンディング サイトです。ページを build 時に static HTML として出力する単一 Astro app で、Drizzle ORM を使って frontmatter からローカル SQLite database に直接 query します。別の backend API や client-side UI framework はありません。再利用するデータ アクセス helper は `src/lib/` にあり、少量の Astro `<script>` と標準 DOM API で対話性を追加します。

### Web サイトを起動する

1. 前の演習後の `main` ブランチにいることを確認し、Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) で terminal を開きます。
3. フィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro server の準備ができると、次のような表示が出ます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開く dialog が表示された場合は、**x** を選択して閉じます。

5. terminal の `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択してサイトを開きます。

> [!NOTE]
> Codespace terminal で localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Codespace の Web server への private tunnel です。

### Web サイトを確認する

- **Home Page**: タイトル、画像、説明とともに board game の一覧を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどを含む詳細ページを表示します。

## Copilot で backlog を確認する

> [!TIP]
> **Copilot Chat を開く**
>
> Copilot Chat panel を開いて **New Chat** を選択し、新しい会話を始めます。mode と model は演習によって異なります。

project template からリポジトリを作ると、Issue backlog が自動的に作成されています。Copilot に内容を表示するよう依頼します。

1. Chat view の agent dropdown で **Agent** を選択します。Agent は複数 file の変更を計画して実行し、terminal command と tool を使えます。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

2. 使用可能な model から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> この model の選択は、特定の model を推奨するものではありません。ワークショップ作成時に使用した model を記載しています。LLM は確率的なため、提案内容が手順と異なる場合があります。これは想定された動作です。

> [!NOTE]
> LLM の確率的な性質により Copilot は別の MCP command を使う場合がありますが、タスクは完了できるはずです。

3. 次の prompt で backlog を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して Issue 一覧取得を実行します。
5. Copilot が作成した一覧と優先順位を確認します。

## Instruction file をレビューする

Copilot にコード生成を依頼する前に、project の context を伝える [UI instruction file](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) を確認します。

1. `.github/instructions/ui.instructions.md` を開きます。
2. architecture、component 設計、testability、accessibility の原則と、Astro、Tailwind CSS、Drizzle などの file 別 instruction へのリンクを確認します。

> [!TIP]
> instruction file からほかの instruction や project file を参照できます。パスは instruction file からの相対パスです。これによりガイダンスを再利用し、複雑な内容を小さく分け、例や template を示せます。

## フィルタリング機能を実装する

少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) にフィルタリング ロジックを追加または改善する。
- フィルタリング動作の test を追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

また、merge 前に test を実行し、成功させる必要があります。

1. 現在の Copilot 会話を続けるか、**New Chat** で新しい会話を始めます。
2. **Add Context** → **Instructions** → **ui** を選びます。

   ![UI instruction file を選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat view で **Agent** が選択されていることを確認します。
4. model が **Claude Sonnet 4.5** であることを確認します。
5. backlog の関連 Issue に基づき、次の prompt で実装を依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot が project、データ レイヤー、UI、test を調べ、file を変更して test を実行する様子を確認します。

   ![Copilot が project file を調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 作業には数分かかることがあります。editor に **Keep** と **Undo** が表示されても、作業中に選択する必要はありません。Copilot が完了すると、変更ごとに選択肢が表示されます。

7. Copilot に求められたら **Continue** を選択し、test を実行します。

   ![Copilot Chat pane で test の実行確認を求める dialog](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理中に一時停止したり test が失敗したりしても問題ありません。Copilot はコード生成と test を繰り返し、エラーがない状態を目指します。
9. 生成されたコードに問題がないか確認します。

   ![Copilot Agent Mode の完了した Chat session](../../../_images/ex3-agent-mode-proposed-changes.png)

> [!CAUTION]
> Copilot などの AI tool が生成したコードは、必ずレビューしてください。

10. Web サイトを開いて新機能を試します。
11. 動作とコードを確認したら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode で Tailspin Toys の機能を追加する方法を確認しました。

- UI とデータ レイヤーにまたがる新機能を実装しました。
- project を調べ、関連 file を特定して一連の変更を行いました。
- codebase に merge する前に生成された変更と test をレビューしました。

次は [Playwright MCP server で機能をテストし][next-lesson]、プル リクエストを作成します。

### ボーナス演習 — ページ分割を実装する

ゲーム一覧の増加に備え、Copilot にページ分割を実装させます。

- 既存の instruction file など、プロジェクトのベスト プラクティスに従います。
- ユーザーがページ サイズを選べるか、固定値にするかを検討します。
- 希望する実装に必要なガイダンスを prompt に含めます。
- Copilot と反復し、必要な変更や context を伝えます。

## リソース

- [Copilot の ask、edit、agent mode: 役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
