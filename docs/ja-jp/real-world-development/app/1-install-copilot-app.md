---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップアプリケーションです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分だけで作業するのではなく、複数のエージェントをそれぞれ分離されたワークスペースで並行して動かし、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js をインストールし、プロジェクトのコピーを用意できたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートで用意されたバックログを含め、ワークスペースを確認します。
- クイックチャットを試して、アプリについて質問します。

## シナリオ

チームは増え続けるバックログに取り組むため、AI エージェントを導入します。Copilot app を使うと、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージなどを一か所から指示できます。このレッスンではアプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選び、画面の指示に従って認証します。データレジデンシー対応の GitHub Enterprise Cloud を使う場合は、**Use GitHub Enterprise** を選び、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディング中にリポジトリの選択が表示されたら、テンプレートから作成した Tailspin Toys リポジトリを選びます。リポジトリ選択をスキップし、オンボーディング後に追加することもできます。
6. テーマの選択を求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** にリポジトリが表示されていることを確認します。すでに表示されていれば、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで記録した owner と repository name を入力します。
9. リポジトリがサイドバーに表示されることを確認します。この画面では `tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとめられ、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには次の領域があります。

- **New** — 新しい Copilot チャットセッションを開始します。
- **Pull requests** — アプリの GitHub ネイティブ統合でプルリクエストを表示します。アプリを離れずに PR を参照、絞り込み、CI 状態の確認、レビューができます。
- **Issues** — Issue を参照、絞り込み、Issue からセッションを開始できます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。やることリストの管理、定期的な保守、その他の面倒な作業を任せるのに役立ちます。振り返りでは次のステップとして紹介し、別のワークショップ演習にはしません。
- **Customize** — MCP server、プラグイン、スキル、その他のコンポーネントを追加し、Copilot app の機能を拡張します。Playwright MCP の設定に使います。
- **Chats** — 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの簡易な会話です。最初のチャットを開始すると表示されます。表示されていない場合は **Sessions** の横にある **+** を選び、最初のチャットを開始します。
- **Sessions** — リポジトリフォルダーとエージェントのセッションがまとめられています。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリビューが開き、その下のセッションを選ぶと会話に戻ります。セッションごとに分離されたワークスペースで実行されるため、変更が競合せずに複数のセッションを同時に実行できます。星評価を追加するときに最初のセッションを開始します。

ワークショップを進めながら、ワークスペースの各機能を確認します。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法が分からない場合や、何かが可能かどうか知りたい場合に、Copilot が案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートを作成したときに用意されたバックログを確認しましょう。

1. サイドバーの **Sessions** で Tailspin Toys のリポジトリフォルダーを選びます。フォルダーの下にある個別のセッションではありません。次に、リポジトリ上部のナビゲーションから **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルで探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue はエージェントセッションの開始点にもなります。小さな最初の変更を終えた後、フィルタリングの Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。ほかのリポジトリの作業を表示するには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問してみましょう。**quick chat** は、ブランチや worktree を作らずに質問やブレインストーミングができるため、セッションを必要としない、その場限りの簡単な質問に適しています。

1. サイドバーで **Chats** の横にある **+** を選び、新しいチャットを開きます。**Chats** がまだ表示されていない場合は **Sessions** の横にある **+** を選び、最初のチャットを開始します。開始後に **Chats** が表示されます。
2. アプリのセッションについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションは分離された独自の git worktree で実行されるため、変更が競合せずに複数のエージェントを並行して実行できます。会話はいつでも続けられ、新しいチャットも開始できます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** から用意されたバックログを見つける。
- クイックチャットで、その場限りの簡単な質問をする。

次は[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使い始め方][getting-started]
- [GitHub Copilot app でエージェントセッションを使う][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
