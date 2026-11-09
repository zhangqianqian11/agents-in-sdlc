---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys のコピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な環境を準備します。Tailspin Toys repository のコピーを作成し、[Codespace][codespaces] を起動します。次のレッスンでは、統合 terminal から Copilot CLI をインストールして実行します。

このレッスンでは、次のことを行います。

- template から Tailspin Toys project のコピーを作成します。
- Codespace を作成し、project の準備ができていることを確認します。

## 演習用リポジトリをセットアップする

[template repository][tailspin-template] から自分用の Tailspin Toys project を作成します。新しい repository には lab に必要な file がすべて含まれています。

1. 新しい browser window で [Tailspin Toys template][tailspin-template] を開きます。
2. **Use this template**、続いて **Create a new repository** を選択してコピーを作成します。
3. GitHub または Microsoft 主催のイベントで参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる organization に repository を作成します。
4. 作成した repository のパス（`organization-or-user-name/repository-name`）を控えます。ワークショップ中に参照します。

> [!NOTE]
> template から repository を作ると、GitHub Issue backlog も自動で作成されます。ワークショップではこの Issue を使うため、自分で登録する必要はありません。

ワークショップ template の新しいコピーを使ってください。repository instruction、アプリケーション code、test、`quality-checks` skill、backlog が含まれています。古いコピーを使う場合は、必要な file が含まれていることを進行担当者に確認してください。

## Codespace を作成する

次は Codespace でワークショップを進めます。[GitHub Codespaces][codespaces] は、browser からコードを作成、実行、debug できる cloud 開発環境です。複数の programming language、extension、tool に対応する editor を提供します。

1. 作成した repository を開きます。
2. **Code** を選択します。
3. **Codespaces** タブを選び、**Create codespace on main** を選択します。
4. Codespace のセットアップ完了を待ちます。template が project dependency、Playwright Chromium、local database をインストールします。
5. **Do you trust the authors of the files in this folder?** と尋ねられた場合は、**Trust Folder & Continue** を選択します。
6. repository root で terminal を開き、app を起動します。

   ```bash
   npm run dev
   ```

7. Codespaces で port `4321` が利用可能になったら **Open in Browser** を選択し、Tailspin Toys site が開くことを確認します。
8. terminal に戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発 server を停止します。

## まとめと次のステップ

環境の準備ができました。

- template から Tailspin Toys project のコピーを作成しました。
- Codespace を作成し、project の準備ができていることを確認しました。

次は Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub account で認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template から repository を作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
