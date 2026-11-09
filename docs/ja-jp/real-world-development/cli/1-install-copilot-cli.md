---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みのフィルタリング Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、terminal で動作する agent 型コーディング assistant です。codebase の調査、コード生成、command 実行、外部 tool との連携を command line から行えます。task を任せたり、変更を依頼したりできます。まず tool をインストールします。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub account で認証します。
- workshop repository を信頼し、簡単な会話を試します。
- 組み込み GitHub MCP server でフィルタリング Issue を探します。

## シナリオ

チームは増え続ける backlog に取り組むため、AI agent を使い始めています。Copilot CLI を使うと、多くの開発者が利用する terminal で agent を使えます。このレッスンでは、インストールと認証を完了し、残りの workshop で使えるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js が事前インストールされているため、npm を使います。

1. Codespace に戻り、terminal を開きます。
2. Node.js がインストールされ、必要な version であることを確認します。

   ```bash
   node --version
   ```

   version 24 以上が表示されます。

3. Copilot CLI を global にインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   version number が表示されます。

## GitHub で認証する

初回起動時に Copilot CLI は GitHub account での認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められたら、device code の手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次の prompt が表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. path が Tailspin Toys repository であることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では GitHub session により、すでに認証されている場合があります。Copilot CLI が認証を尋ねずに起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常の shell prompt で入力する command は Codespace で直接実行されます。Copilot CLI の起動後は、自然言語を agent に送信し、slash command で会話を操作します。

1. `/model` を入力します。矢印キーで **Auto** を選択し、<kbd>Enter</kbd> を 2 回押して確定します。
2. `/help` で現在の version の command を確認し、<kbd>Esc</kbd> で閉じます。
3. 次の質問で動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が答える前に repository を調査する様子を確認します。
5. `/mcp list` を入力し、組み込み GitHub MCP server が使えることを確認します。
6. 次の prompt でフィルタリング Issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き、Issue を読みます。最初の変更が終わった後に使います。

> [!TIP]
> 通常の Copilot CLI session は、terminal で現在 checkout されている branch で作業し、自動的に worktree は作成しません。変更を始める前に feature branch を作ります。

## Workshop 用 shortcut を使う

通常、Copilot CLI は許可された範囲外の tool を使う前に確認します。この workshop では Codespace 内で承認 prompt を省略し、演習に集中できるよう `--yolo` を付けて再起動します。

> [!CAUTION]
> `--yolo` は完全な自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM のような分離された環境でのみ使い、普段の開発の default として alias に設定しないでください。詳細は [Allowing and denying tool use][allow-all-warning] を参照してください。

`--enable-all-github-mcp-tools` は、後続レッスンで Issue や PR を扱うための GitHub MCP read/write tool を有効にします。Codespace は local computer へのアクセスを制限しますが、認証された GitHub resource は実際のものです。公開や merge の前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. repository root から再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. project について簡単な質問を送り、会話が動くことを確認したら `/exit` で終了します。

Copilot は会話を自動的に保存します。instruction を変更したり agent を追加したりした後、`copilot --resume` で同じ会話と branch に戻ります。

## まとめと次のステップ

- npm で GitHub Copilot CLI をインストールしました。
- GitHub account で認証しました。
- workshop repository を信頼し、会話を試しました。
- 組み込み GitHub MCP server でフィルタリング Issue を見つけました。

次は [最初の小さな変更を始め][next-lesson]、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
