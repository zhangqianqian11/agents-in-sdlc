---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。[template][template-repository] から Tailspin Toys リポジトリのコピーを作成し、instruction file の編集や cloud agent の成果物レビューに使う [Codespace][codespaces] を起動します。

## 演習用リポジトリをセットアップする

template から新しいリポジトリを作成すると、演習に必要なファイルが揃ったコピーを作れます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys template][tailspin-template] を開きます。
2. リポジトリのページで **Use this template** を選択し、次に **Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントで参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![リポジトリ template の設定](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えます。演習中に参照します。

## Codespace を作成する

続いて、演習で使う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーでコードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、tool に対応する IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンで新しい Codespace を作成します。

   ![新しい Codespace を作成](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手作業でインストールするよりも短時間で準備できます。その間に GitHub Copilot の機能を確認してみてください。

> [!CAUTION]
> 今後の演習でも Codespace を使います。いったん、ブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作するように構成されています。どちらの環境にも、必要な前提条件がインストールされています。ローカルで実行する場合は、clone したリポジトリを VS Code で開き、表示されたときに **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

演習用リポジトリのコピーを作成し、Copilot cloud agent と一緒に作業する Codespace を準備しました。

## 次のステップ

cloud agent が従う custom instruction を追加します。[演習 1 - custom instruction][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [template からリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
