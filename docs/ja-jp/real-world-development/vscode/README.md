---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** を使うと、普段使っているコードエディターで GitHub Copilot を利用できます。Visual Studio Code や GitHub Codespaces で Copilot Chat のエージェントモードを使い、MCP 経由で外部ツールに接続し、カスタムエージェントを利用できます。IDE を離れずに、ファイル、ターミナル、問題をすべて Copilot に参照させながら作業できます。

まずカスタム指示を追加して、Copilot がそれに従うことを確認します。次にエージェントモードで UI、データレイヤー、テストにまたがるフィルタリング機能を構築します。Playwright MCP server を接続し、プルリクエストを開く前に Copilot にブラウザーを操作させ、機能をテストします。最後に、アクセシビリティ向けのカスタムエージェントを確認して使用し、エディターを離れずに Copilot の変更を監視、誘導して改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | VS Code でカスタム指示を追加して確認する |
| [2. Agent Mode][ex2] | コード生成 | agent mode でフィルタリング機能を構築する |
| [3. Playwright を使った MCP][ex3] | 外部ツール | Playwright MCP server でブラウザーから機能をテストする |
| [4. カスタムエージェント][ex4] | 専門エージェント | カスタムエージェントを確認して使う |
| [5. エージェントの管理][ex5] | 監視 | エージェントのセッションを監視して誘導する |
| [6. 反復して改善する][ex6] | レビュー | Copilot の作業をローカルでレビューし、次のステップを決める |
| [オプション: Foundry の組み込み][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit を使い、3つのモジュールでモデルを準備し、エージェントをデプロイしてサイトに接続する |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかが有効な GitHub アカウント
- [ ] GitHub Codespaces へのアクセス

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
