---
title: "レッスン 2 - 星評価を追加する: 小さな成果"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試したので、次は**エージェントセッション**を開始して、プロジェクトに最初の変更を加えます。変更は小さなものにします。ゲームデータにはすでに星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェントセッションを開始し、セッションの構成を学びます。
- エージェントに小さく目的を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリ、発行者、説明だけが表示されています。最初のセッションの準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。小さく自己完結した、最初のセッションに適した変更です。

## セッションの構造

**セッション**は、分離された独自のワークスペースで実行されるエージェントとの会話です。すべてのセッションに専用の **git worktree とブランチ**が割り当てられます。そのため、一方で機能を追加し、もう一方でバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにグループ化されてサイドバーに表示されます。セッションを選ぶと切り替えられます。

セッション内には、エージェントとの**会話**、ファイルの調査や編集を行うエージェントの**ツール操作**、差分付きの**変更ファイル一覧**が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトを調べて機能を実装します。[アプリのセットアップ][prior-lesson]で GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用に新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選び、セッションを開始します。
3. リポジトリに `tailspin-toys` を選びます。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選び、次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> キーを押して、Copilot にプロンプトを送信します。

Copilot app はまず新しい worktree を作成し、分離されたプロジェクトのコピーで作業を始めます。次にプロジェクトを調べ、新機能に必要な更新対象ファイルを見つけて、必要なコードを作成します。これで Copilot app を使って新しい機能を追加できました。

## 差分をレビューする

小さな変更であっても、AI が生成した変更はマージする前にすべてレビューします。Copilot app 内で変更を確認しましょう。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルと差分を確認します。

   ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームカードを表示する中心的なファイル `GameCard.astro` に、次のようなコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は `No rating yet` を表示する、小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、実際のコードは上記と異なる場合があります。おおむね同じ内容になります。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果をレビューします。数値の `starRating` と `null` の場合の表示をテストしていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認してください。

コードを読むだけで動作すると決めつけず、Copilot に Web サイトを起動してもらい、更新された UI を確認しましょう。Web サイトを起動し、ブラウザーキャンバスで開くよう依頼します。

> [!TIP]
> キャンバスは、Copilot app 内で使用できるインタラクティブなウィジェットです。後でカスタムキャンバスを試して作成しますが、今は組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトで、アプリを起動してブラウザーキャンバスで開くよう Copilot に依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーウィンドウが開きます。
3. 評価済みのゲームカードに5点満点の値が表示されることを確認します。
4. 確認が終わったら、次のプロンプトでこのセッションが起動した開発サーバーを停止するよう Copilot に依頼します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

機能ができました。次は、新しいコードを既存のコードベースにマージするためのプルリクエスト (PR) を作成します。

1. 右上隅の **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選び、画面の指示に従って認証します。
3. PR を作成する前に、関連 Issue が必要なリポジトリポリシーが Copilot から説明された場合は、その手順に従うことを確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビュー用のペインで PR を開きます。必要に応じてここでレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースしました。具体的には、次のことを行いました。

- エージェントセッションを開始し、セッションの構成を学びました。
- エージェントにゲームカードの小さく目的を絞った変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、明示的にマージしました。

次は、[フィルタリングの Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
