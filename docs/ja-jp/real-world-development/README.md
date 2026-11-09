---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を計画し、コードを作成して動作をテストし、プル リクエストをレビューし、agent と作業を管理します。

## シナリオ

架空の企業 Tailspin Toys に新しく加わった開発者として、ボードゲームのクラウドファンディングを提供するサイトを担当します。チームのバックログは GitHub Issue として登録済みです。カテゴリやページネーションなどの機能に加え、アクセシビリティやコーディング標準の改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。開発方法に合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルで agent 型アシスタントを使います。Plan と Autopilot の各モード、instruction、skill、custom agent、Playwright MCP、Agent Merge、実用的な slash command を扱います。

### [GitHub Copilot app][app]

デスクトップ アプリで agent セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue とプル リクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期 agent に割り当て、作業を案内して進捗を監視し、作成されたプル リクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat の agent mode、MCP server、custom agent を操作します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
