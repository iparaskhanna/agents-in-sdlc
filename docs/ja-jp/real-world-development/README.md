---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を試します。反復しながら変更を計画し、コードを作成して動作をテストし、プルリクエストをレビューし、エージェントを使って作業を管理します。

## シナリオ

開発者向けテーマのボードゲーム クラウドファンディングを提供する架空の会社、Tailspin Toys に新しく加わった開発者として作業します。チームのバックログは GitHub Issue として登録済みで、すぐに取り組めます。機能追加にはフィルタリングやページネーション、品質改善にはアクセシビリティやコーディング標準が含まれています。

## 作業環境を選ぶ

GitHub Copilot は、どこで作業していても利用できます。開発方法に合った環境を選んでください。各ワークショップでは、それぞれの環境を準備し、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実践的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内して進捗を監視し、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
