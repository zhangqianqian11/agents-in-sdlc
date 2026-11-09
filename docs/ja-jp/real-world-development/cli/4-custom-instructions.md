---
title: "レッスン 4 - custom instruction で Copilot を導く"
description: "リポジトリの instruction を確認してドキュメント標準を追加し、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要があるなら、そのガイダンスを Copilot が参照できるようにします。[instruction file][instruction-files] では、必要なコードだけでなく、その構成方法も説明できます。フィルタリングを実装したので、Copilot が参照した instruction を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリ instruction とパス単位の instruction が agent に適用される方法を確認します。
- コーディング標準を守るため、instruction を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインがあります。

- コメントはコードの繰り返しではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` の export 関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記載します。該当する場合は、注入可能な `db` 引数も説明します。
- 再利用可能な Astro component には `Props` の契約を記載し、関連コードが変わったらコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

instruction file を使い、これらのプラクティスに沿ってタスクを進めるための情報を Copilot に渡します。

## Instruction file

custom instruction を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用するものと、特定のファイル種別に適用するものを作成できます。

instruction file には 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の instruction file です。ほとんどのリクエストに関係するプロジェクト レベルのコンテキストを記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインです。

> [!NOTE]
> ほかの instruction 形式や各環境でのサポート状況は異なります。特定の形式に依存する前に、[custom instruction のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトの custom instruction file を確認する

スターター プロジェクトには instruction file が用意されています。変更前に確認し、変更後の効果と比較します。

1. Codespace に戻ります。
2. Codespaces editor で（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この instruction は、このリポジトリ内で Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの instruction を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob です。
6. `.github/instructions/drizzle.instructions.md` のリンク先も確認します。instruction を再利用可能な小さな単位に分け、Copilot が参照できる例を示せます。

## チームのガイダンスに合わせて instruction を更新する

`.github/copilot-instructions.md` を更新し、新しく生成される TypeScript に TSDoc comment が付くようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 最後の箇条書きに次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリ instruction を読み込みます。instruction の更新後、フィルタリングの会話を終了して再開すると、機能のコンテキストを維持したまま新しいガイダンスを読み込めます。

1. フィルタリングの会話が開いている場合は `/exit` で終了します。会話は自動保存されます。
2. リポジトリの root で `game-filters-cli` ブランチに切り替わっていることを確認し、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に更新後のガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して変更された TypeScript ファイルを確認します。追加された TSDoc comment がコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が instruction file からコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。

- リポジトリ instruction とパス単位の instruction を確認しました。
- instruction file を更新してコーディング標準を指定しました。
- instruction file がコードに与える影響を確認しました。

次は、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、テストと lint を一貫して実行します。

## リソース

- [Copilot CLI に custom instruction を追加する][instruction-files]
- [custom instruction のサポート][custom-instructions-support]
- [custom instruction 作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
