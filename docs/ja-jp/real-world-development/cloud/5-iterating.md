---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent の管理][previous-lesson]

## 作業をレビューする

この lab では、ユーザー体験の改善と機能追加について GitHub Copilot に複数の作業を依頼しました。コードへのドキュメント追加、関連ゲーム機能の実装、ハイ コントラストとライト モードを含むアクセシビリティ機能です。コード変更を確認し、必要なら Copilot に改善を依頼します。

### シナリオ

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体に review を有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監視なしで作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取り権限を持ち、書き込みは作業ブランチに限定されます。
- cloud agent は GitHub Actions 内に一時的な独立環境を作成して実行されます。
- GitHub Actions workflow の実行には、人間による承認が必要です。
- MCP server などの[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初のプル リクエスト (PR) は、コードにドキュメントを追加する変更です。github.com の標準 PR interface で確認します。

> [!NOTE]
> PR に GitHub Copilot が firewall によりブロックされたという警告が出る場合があります。Copilot は MCP server を含め、外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent の firewall を設定または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻り、**Pull Requests** を選択します。
2. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR に **[WIP]** が表示されます。完了するまで待ってください。数分かかることがあります。

3. **Files changed** タブを選び、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

4. 追加された TSDoc doc comment やドキュメントを確認します。変更内容は毎回異なることがあります。Exercise 2 で確認した custom instruction に基づく TSDoc、TypeScript 規約、ファイル コメント ヘッダーがあるかを見ます。
5. **Conversation** タブに戻って下にスクロールし、workflow の承認待ちを確認します。承認待ちなら **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

6. PR の checks に workflow が追加されたことを確認します。すべて成功すると、単一 Astro app の project check が通過します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

PR のコメントやコードの inline comment でチーム メンバーと同じように **@copilot** をタグ付けして、更新を依頼できます。Copilot はコメントを確認し、別の session を開始して対応します。結果は確率的に変わるため、固定の prompt はありません。例:

- 各コード file の先頭に、役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro file に TSDoc doc comment を追加する。
- Astro app の構造を説明する README を作る。

1. 上記の例などを使い、生成されたドキュメントに変更を依頼するコメントを **@copilot** とともに追加します。
2. **View Session** を選択して Copilot の作業を確認します。更新用の新しい session が始まります。
3. **Back to pull request** を選択して PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業完了後、PR に新しい commit が追加されたことを確認します。
5. **Files changed** タブで変更をレビューします。必要に応じて納得できるまで反復します。準備ができたら draft を review 可能にして、main にマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue の PR を確認します。

1. リポジトリの **Pull Requests** タブを開きます。
2. **Show related games on the game details page** などの PR を選択し、**Files changed** タブを確認します。
3. 問題がなければ **Conversation** タブに戻り、workflow の承認待ちを確認します。必要に応じて **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

4. checks で単一 Astro app の project check が成功したことを確認します。
5. **任意:** Codespace でこのブランチに切り替えて機能を手動テストできます。`<branch-name>` を Copilot が作成した branch 名（例: **copilot/fix-8**）に置き換えて実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

必要ならコメントを追加して Copilot に変更を依頼します。たとえば表示する関連ゲームの数、コメント ヘッダー、TSDoc doc comment などを調整します。この Issue は instruction 更新前に割り当てたため、instruction が反映されていない場合があります。**Conversation** タブのコメントから Copilot に依頼します。

## アクセシビリティ機能をレビューする

最後に、accessibility custom agent を使って実装した機能を確認します。この PR には Exercise 3 で依頼したハイ コントラスト モードと、Exercise 4 で追加したライト モードが含まれます。

1. **Pull Requests** タブで **Add high contrast mode to website** などの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR に **[WIP]** が表示されます。完了するまで待ちます。

2. **Files changed** タブで実装をレビューします。特に次の点を確認します。

   - モード切り替え用の UI component。
   - ユーザー設定の保存に使う local storage。
   - ハイ コントラスト モードとライト モードの CSS / style。
   - ARIA label や keyboard navigation などのアクセシビリティ属性。
   - 表示モードを切り替える JavaScript / TypeScript。

3. 問題がなければ **Conversation** タブに戻り、workflow の承認待ちを確認します。必要に応じて **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

4. checks で project check の成功を確認します。
5. **任意:** Codespace でブランチに切り替えて手動テストします。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、ハイ コントラスト モードとライト モードの切り替えを試します。ページを再読み込みしても設定が維持されることを確認します。

accessibility custom agent がアクセシビリティのベスト プラクティスに沿って Copilot の作業を導いたことを確認します。改善点があれば、ほかの PR と同様にコメントで **@copilot** に更新を依頼できます。

## オプションの演習 — 委任を続ける

cloud agent に実際のバックログ作業を任せ、その間に別の作業を進める方法を試します。Issue を作って Copilot に割り当てる例:

- ゲーム詳細ページに backer interest form を作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent harness を完了しました。

- リポジトリに含まれる custom instruction を確認し、cloud agent の出力への影響を確認しました。
- Issue を Copilot cloud agent に割り当て、環境準備、計画、非同期実行を確認しました。
- accessibility custom agent を作成して使い、ハイ コントラストとライト モードを追加しました。
- Copilot agents page を mission control として使い、実行中の session を監視して方向を調整しました。
- cloud agent の PR をレビューして反復し、**@copilot** に変更を依頼し、workflow を承認しました。

## レビューと次のステップ

Cloud agent harness は完了です。ほかの harness では、今回扱った内容を補完する作業を体験できます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE 内で Copilot Chat agent mode と MCP integration を使います。
- 💻 **[CLI harness](../../cli/)** — Plan、Autopilot、agent skill、QA agent、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルから 2 つの変更をリリースします。

自分のリポジトリでも次のことを試してください。

- refactoring またはテスト範囲の Issue を cloud agent に割り当てます。
- 別の分野の custom agent を作成します。
- 異なる stack 用に `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent setup workflow のベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent firewall の設定][agent-firewall]

---

| [← 前のレッスン: Agent の管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
