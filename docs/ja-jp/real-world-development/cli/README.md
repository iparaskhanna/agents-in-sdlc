---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys の変更を2つ実装、検証してリリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型のコーディングアシスタントとして利用できます。コマンドラインからコードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できるため、グラフィカルエディターに切り替えずに作業の流れを保てます。

このワークショップでは、Tailspin Toys の一連のワークフローを体験します。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトを確認します。
2. 星評価の小さな変更を加え、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装して、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルタリングの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) server を追加し、ブラウザーでフィルタリングを確認します。
7. 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、最終検証の証拠を確認します。
8. フィルタリングの変更全体をレビューし、PR に Agent Merge を使用します。
9. コンテキスト、モデル、共有、cloud agent への任意の委任に役立つスラッシュコマンドを確認します。

ワークショップでは、星評価の PR と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルタリングの PR の2つを作成します。フィルタリングと品質確認のワークフローでは、同じ会話とブランチを使い、各ツールを学びながら作業を積み重ねます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本操作を確認する |
| [2. 星評価を追加する: 小さな成果][ex2] | 最初の変更 | 既存の評価と未評価の場合の表示を追加し、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して Interactive モードでレビューする |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認、更新し、フィルタリングに適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定し、フィルタリングの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門家プロファイルを作成して選択し、最終検証の証拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューして PR を作成し、Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、参考リソースを振り返る |
| [オプション: Foundry の組み込み][foundry] | ホスト型エージェント | モデルを準備し、カタログの情報に基づくエージェントをデプロイしてサイトに接続する |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかが有効な GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドラインの基本操作に関する知識

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を利用する場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
