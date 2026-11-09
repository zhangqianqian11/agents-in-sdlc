---
title: "演習 1 - custom instruction (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法で進める必要があるタスクや、Copilot に伝えておくべき背景情報がある場合は、参照できるようにします。[instruction file][instruction-files] は、コード生成の指針を Copilot に伝える方法の 1 つです。

この演習では、次のことを行います。

- リポジトリ custom instruction とパス単位の instruction file を使って、プロジェクトのコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に渡る方法を確認します。
- 既存の instruction でフィルタリング用 publishers helper を生成します。
- `.github/copilot-instructions.md` に新しい標準を追加します。
- 同じ prompt を再実行し、生成コードに新しい標準が反映されることを確認します。
- instruction の更新と helper を `main` に commit し、次の演習に引き継ぎます。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、instruction の更新後に動作の傾向が変わることを確認します。出力が 1 文字ずつ一致することが目的ではありません。

## Instruction file

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- データ レイヤーには必ず unit test を追加します。
- UI はダーク モードで、モダンな印象にします。
- TSDoc doc comment を使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、役割を説明するコメント ブロックを追加します。

instruction file を使い、これらのプラクティスに沿うために必要な情報を Copilot に渡します。

### Custom instruction

custom instruction を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコードを得られます。リポジトリ全体または特定のファイル種別に適用する instruction を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の instruction file です。技術スタック、プロジェクト概要、ベスト プラクティスなど、プロジェクト全体に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクごとのガイドラインを記載します。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction と、`applyTo` frontmatter を持つ `*.instructions.md` file の両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの形式の instruction file][custom-instructions-support] もサポートしています。

### Instruction file を管理するためのベスト プラクティス

- `copilot-instructions.md` は、プロジェクトの概要、構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスに使います。
- `*.instructions.md` は、テスト、Astro component、データ レイヤーなどのファイル種別や特定のタスクに使います。
- 自然言語で明確に記述し、望ましいコードと望ましくないコードの例を示します。

instruction file の作り方に唯一の正解はありません。プロジェクトに合う方法を試してください。

> [!TIP]
> instruction file は、[UI 更新][ui-instructions]や [Astro][astro-instructions] などのタスクにも使えます。Copilot に生成させることもできます（例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。テンプレートを探す場合は、instruction、custom agent、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

1. `.github/copilot-instructions.md` を開き、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。
2. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの instruction を見ます。
3. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` glob を確認します。例として `**/*.test.ts` に一致する TypeScript test file が対象です。
4. unit test 作成用の指示を確認します。
5. `.github/instructions/drizzle.instructions.md` の末尾まで読み、ほかの instruction やプロジェクト file へのリンクを確認します。パスは repo root ではなく、この instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** に、コード内ドキュメントの規則はまだありません。次に TSDoc doc comment とファイル コメント ヘッダーの規則を追加します。

## 変更用ブランチを作成する

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) で terminal を開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## Instruction 更新前に Copilot Chat を使う

まず現在の instruction のまま prompt を実行します。その後 instruction を更新し、同じ prompt をもう一度送信して違いを確認します。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat panel を開いて **New Chat** を選び、新しい会話を始めます。mode と model は演習によって異なります。必要な場合は各手順に記載されています。

1. 前の演習の editor tab を閉じます。
2. `src/lib/publishers.ts` を開きます。
3. Chat view の agent dropdown で **Agent** を選択します。

   ![Chat view の agent picker](../../../_images/shared-chat-mode-selector.png)

4. 次の prompt を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot が project を確認してコードを更新します。helper と test file の両方が変更されることがあります。
6. helper が型付き関数で、最初の引数に `db` client を受け取り、型付き publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の規約に基づきます。
7. 生成コードに TSDoc doc comment とファイル コメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、instruction がなくても doc comment を追加する場合があります。その場合も問題ありません。instruction 更新後に一貫性が高まることが重要です。

## 新しいリポジトリ標準を追加する

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements**（27 行目付近）を探します。コード内ドキュメントの規則がまだないことを確認します。
3. 既存の標準のすぐ下に次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ レベルの `.github/copilot-instructions.md` は全体的な標準に、`*.instructions.md` は特定の言語、ファイル種別、タスクに使います。今回追加した doc comment の規則はリポジトリ レベルの標準です。

## Prompt を再実行して変化を確認する

1. Copilot Chat に戻って **New Chat** を選択します。
2. `src/lib/publishers.ts` を開き、Copilot が対象 file に注目できるようにします。
3. 先ほどと**同じ prompt** を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成された file の先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc comment が含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

次の演習で利用できるよう、この更新済み file を保持します。

## Instruction を commit してブランチを push する

1. 新しい terminal で `git status` を実行し、変更を確認します。
2. instruction と helper の変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

   `git status` に追加の変更（test など）があれば、その file も stage します。

3. ブランチを push します。

   ```bash
   git push -u origin custom-instructions
   ```

## Pull request を作成してマージする

instruction と helper の変更を backlog の Issue に関連付けて PR を作成し、マージします。

> [!NOTE]
> この演習では Copilot に GitHub の操作を依頼します。project template には GitHub tool が設定済みです。初回使用時に VS Code から GitHub sign-in を求められた場合は、表示された手順に従います。後の演習では Model Context Protocol について確認します。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) で Copilot Chat view を開き、agent picker で **Agent** を選択します。
3. 現在のブランチから PR を作成し、instruction と publishers helper の変更を説明して関連 Issue を閉じるよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. GitHub の操作を許可するよう Copilot に求められたら、command を確認して適切に **Allow** を選択します。
5. PR 作成後、次を prompt として送信し、PR をマージしてローカル `main` を更新します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub の操作や shell command の許可を求められたら、内容を確認して適切に **Allow** を選択します。

GitHub Copilot の支援で PR を作成してマージできました。

## まとめと次のステップ

instruction file から Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の instruction を使ってコード生成 prompt を送り、結果を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じ prompt を再実行し、新しい標準が生成コードに適用されることを確認しました。
- instruction と helper を `main` に commit し、次の演習で利用できるようにしました。

次は [agent mode][next-lesson] で instruction を使い、コードベース全体にわたる新機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用 custom instruction][personal-instructions]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
