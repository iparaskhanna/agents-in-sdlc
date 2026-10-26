---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は、Copilot CLI を基盤とするデスクトップアプリケーションで、エージェント主導の開発を1つの作業用ワークスペースで実現します。並列エージェントセッション、切り替え可能なセッションモード、共有キャンバス、GitHub Issue とプルリクエストのネイティブ管理機能を備えています。さらに、rebase、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージまでプルリクエストを導く **Agent Merge** も利用できます。

このワークショップでは、Tailspin Toys の一連のワークフローを体験します。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと用意されたバックログを確認します。
2. 星評価の小さな変更を加えてブラウザーでレビューし、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリングの Issue から始め、**Plan** モードで方針を決め、**Autopilot** モードで実装して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) server を追加し、ブラウザーでフィルタリングを確認します。
7. 品質保証 (QA) カスタムエージェントを作成して使い、要件、テスト範囲、検証の証拠をレビューします。
8. フィルタリングの変更全体をレビューし、2つ目の PR に Agent Merge を使います。
9. Canvas が利用できるか確認してから、Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストします。必要な機能を利用できない場合は、ブロックされていることを記録し、読み取り専用の代替方法を使うか、まとめに進みます。

ワークショップでは、星評価の PR と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの PR の2つを作成します。どちらも更新済みの `main` から開始します。フィルタリングと品質確認のワークフローでは、同じセッション、worktree、ブランチを使い、各ツールを学びながら作業を積み重ねます。最後の Canvas 演習は同じセッション内で行い、PR の手順を繰り返さずに、共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成する |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールし、プロジェクトを接続してワークスペースを確認する |
| [2. 星評価を追加する: 小さな成果][ex2] | 最初の変更 | 既存の評価と未評価の場合の表示を追加し、PR 1 をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して Interactive モードでレビューする |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認、更新し、フィルタリングに適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存スキルのレポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize から MCP を設定し、フィルタリングの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門家プロファイルを作成して選択し、最終検証の証拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | フィルタリング、指示、スキル、QA プロファイル、テストをレビューし、2つ目の PR に Agent Merge を使う |
| [9. Canvas を確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストする |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、成果物、参考リソースを振り返る |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかが有効な GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで実行します。そのため、[前提条件の演習][ex0]でアプリのインストール前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者は **GitHub Copilot app** ポリシーを有効にしておく必要があります。これは Copilot CLI ポリシーとは別です。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
