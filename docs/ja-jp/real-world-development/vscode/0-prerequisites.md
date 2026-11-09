---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な環境を準備します。Tailspin Toys repository のコピーを作成し、作業用の [Codespace][codespaces] を起動して、editor で GitHub Copilot Chat が動作することを確認します。

## 演習用リポジトリをセットアップする

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] を開きます。
2. **Use this template** ボタンを選択し、次に **Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft 主催のイベントで参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しい repository を作成します。

   ![リポジトリ template の設定](../../../_images/ex0-repository-settings.png)

4. 作成した repository のパス（**organization-or-user-name/repository-name**）を控えます。演習中に参照します。

> [!NOTE]
> **Backlog の準備**
>
> template から repository を作成すると、GitHub Issue backlog も自動で作成されます。ワークショップではこの Issue を使うため、自分で Issue を作成する必要はありません。

## Codespace を作成する

演習では Codespace を使います。[GitHub Codespaces][codespaces] は、ブラウザーからコードを作成、実行、デバッグできる cloud 開発環境です。複数の programming language、extension、tool に対応する IDE を使えます。

1. 作成した repository を開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンで新しい Codespace を作成します。

   ![新しい Codespace を作成](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかります。サービスを手作業でインストールするよりも早く準備できます。その間に GitHub Copilot の機能を確認してください。

> [!CAUTION]
> 今後の演習でも Codespace に戻ります。ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動くように構成されています。どちらの環境にも必要な前提条件が用意されています。ローカルで実行する場合は、clone した repository を VS Code で開き、求められたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat と agent mode を使う

Copilot Chat agent mode を使うには、IDE に GitHub Copilot Chat extension が必要です。GitHub Codespace を使う場合は、通常すでにインストールされています。

> [!TIP]
> extension がない場合は [Visual Studio Code Marketplace からインストール][copilot-chat-extension]するか、VS Code の Extensions view で **GitHub Copilot Chat** を検索し、**Install** を選びます。

extension をインストールした後、GitHub account で認証が必要になる場合があります。

1. Codespace に戻ります。
2. editor の右側に Copilot Chat が表示されていない場合は、Codespace 上部にある **Copilot Chat** icon を選択します。
3. Copilot Chat window に `Hello world` などの message を入力して <kbd>Enter</kbd> を押します。
4. 認証されていない場合は GitHub account への sign-in が求められます。表示される手順に従って認証します。

   ![Copilot Chat の認証 prompt](../../../_images/ex1-copilot-authentication.png)

5. 認証後に Copilot Chat window が表示されることを確認します。

## まとめ

演習用 repository のコピーを作成し、コード作成に使う Codespace の準備を始めました。

## 次のステップ

次は Copilot に project の規約を伝えます。[演習 1 - custom instruction][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template から repository を作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
