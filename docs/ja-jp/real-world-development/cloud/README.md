---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** は、GitHub Copilot がクラウド上で非同期に作業できる機能です。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べて変更を行い、プル リクエストを作成します。その間、別の作業を続けられます。

演習では、cloud agent が従う custom instruction を追加してから、GitHub Issue を割り当てて実装を依頼します。custom agent を確認して使い、agents dashboard からセッションの進捗を監視して作業の方向を調整し、最後に作成されたプル リクエストをレビューして結果を改善します。

## 演習

| 演習 | トピック | 説明 |
| ------ | ------ | ------ |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. custom instruction][ex1] | コンテキスト | cloud agent が従う custom instruction を追加する |
| [2. Cloud agent][ex2] | 非同期 agent | Issue を Copilot cloud agent に割り当てる |
| [3. Custom agent][ex3] | 専門 agent | custom agent を確認して使用する |
| [4. Agent の管理][ex4] | 監視 | agent セッションを監視して作業の方向を調整する |
| [5. 反復作業][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] アカウントまたは組織で Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コード レビュー、Copilot CLI の機能が含まれています。

[student-plan]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free 以外の **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> この harness では MCP を扱いません。Copilot で MCP server を使う方法については、[CLI harness](../cli/) または [VS Code harness](../vscode/) を参照してください。

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
