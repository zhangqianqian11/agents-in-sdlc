---
title: "演習 1 - custom instruction (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に伝えておくべき背景情報がある場合は、その情報を参照できるようにします。[instruction file][instruction-files] を使うと、Copilot に実行内容だけでなく方法も伝えられます。

この演習では、次のことを行います。

- リポジトリ custom instruction とパス単位の instruction file を通じて、プロジェクトのコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に渡る方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI harness と違い、ここでは prompt の変更前後を比較しません。Copilot cloud agent は GitHub Issue で非同期に動くため、リアルタイムで並べて効果を示すのが難しいためです。この harness の後半で、instruction が cloud agent の PR に与えた影響を確認します。

## Instruction file

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- データ レイヤーには必ず unit test を追加します。
- UI はダーク モードで、モダンな印象にします。
- TSDoc の doc comment をコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

instruction file を使い、これらのプラクティスに沿うために必要な情報を Copilot に渡します。

### Custom instruction

custom instruction を使うと、コーディング スタイルや要件などのコンテキストを Copilot に伝えられます。リポジトリ全体または特定のファイル種別に適用する instruction を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の instruction file です。技術スタック、プロジェクト概要、ベスト プラクティスなど、広く関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けの instruction です。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの instruction と `applyTo` frontmatter を持つ `*.instructions.md` file の両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの形式の instruction file][custom-instructions-support] もサポートしています。

### Instruction file を管理するためのベスト プラクティス

サンプル プロジェクトには、一般的な管理方法を示す例が含まれています。

- `copilot-instructions.md` は、プロジェクト構造や全体的なコーディング標準など、プロジェクト レベルのガイダンスに使います。
- `*.instructions.md` は、unit test、Astro component、データ レイヤーなど特定のファイル種別やタスクに使います。
- 明確な自然言語を使い、望ましいコードと望ましくないコードの例を示します。

instruction file の作り方に唯一の正解はありません。実際に試してプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した instruction file のセットを用意します。このプロジェクトには [UI 更新][ui-instructions] や [Astro][astro-instructions] などの instruction があります。
>
> Copilot は instruction file の生成も支援できます。各 surface での手順は異なります（例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。テンプレートを探す場合は、instruction file や custom agent の例がある [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの custom instruction file を確認する

このリポジトリには、中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md` が含まれています。エディターまたは GitHub web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** の各セクションを確認します。**Code standards** の **GitHub Actions Workflows** のガイダンスも確認してください。
3. `.github/instructions` フォルダーを見て、Astro、Drizzle データ レイヤー、テストなどの instruction を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` が対象ファイルを指定する glob であることを確認します。たとえば `**/*.test.ts` に一致する TypeScript test file が対象です。
5. unit test 作成用の instruction を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾まで読み、ほかの instruction file やプロジェクト内の file へのリンクを確認します。instruction を再利用可能な単位に分け、例を示せます。（記載されたパスは repo root ではなく、この instruction file からの相対パスです。）

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次に TSDoc doc comment とファイル コメント ヘッダーの規則を追加します。

## 新しいリポジトリ標準を追加する

cloud agent が後の演習で Issue に取り組むときに、この標準を使えるようにします。

1. Codespace の terminal で、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き直します。
3. **Code formatting requirements** セクション（27 行目付近）を探します。
4. 既存の標準のすぐ下に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc doc comment を指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> instruction file は、全体的なガイダンス向けのリポジトリ レベル (`.github/copilot-instructions.md`) にも、特定の言語やタスク向けの `*.instructions.md` にも作成できます。プロジェクト全体の標準はリポジトリ レベルの file に記載します。

## Instruction の更新を commit、push、merge する

cloud agent は Issue の対象ブランチにある instruction file を読みます。次の演習で agent に Issue を割り当てると、`main` からブランチを作るため、instruction の変更を `main` に反映します。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への PR を作成してマージします。次の演習で cloud agent がこれらの instruction を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを作成する手順を省略して `main` に commit できます。ブランチの手順は、実際のプロジェクトでの進め方を示しています。

## まとめと次のステップ

instruction file から Copilot がコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を追加しました。cloud agent が後の演習で作成する PR に、この標準が反映されます。

次は [Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の custom instruction file][instruction-files]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向け custom instruction を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
