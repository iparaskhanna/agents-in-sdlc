---
title: "レッスン 2 - 星評価を追加する: 小さな成果"
description: "Copilot CLI でゲームカードに小さな変更を加え、ポート転送したブラウザーで確認して、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで目的を絞った Copilot の会話を始めます。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送したブラウザーでアプリを実行して確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲームの詳細ページにはすでに表示されています。一方、ホームページのゲームカードにはタイトル、カテゴリ、発行者、説明しか表示されていません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションにぴったりの、小さく独立した変更です。

## 会話のしくみ

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では、専用の worktree を作成するのではなく、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存した会話に戻って同じ話し合いを続けられますが、ファイルとブランチはディスク上の通常の Git 状態のままです。

会話には、プロンプトとエージェントの応答、ファイルを調査・編集するエージェントのツール操作、`/diff` で確認できる変更の3つが表示されます。

## 会話を始めて変更を依頼する

機能の実装を始めるため、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. 次のコマンドを実行して、Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を入力して <kbd>Enter</kbd> キーを押し、新しいセッションを開始します。
5. 次のプロンプトを入力して変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけて、必要なコードを作成します。これで Copilot CLI を使って機能を追加できました。

## 差分をレビューする

小さな変更であっても、AI が生成した変更はマージする前にすべてレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更されたファイルをすべて確認します。
2. 評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方の状態が対象になっていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> キーを押して差分画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作するため、生成されるコードは異なる場合があります。特定の実装を期待するのではなく、動作をレビューしてください。

## 変更を確認する

コードを読むだけで、動作すると決めつけてはいけません。Copilot に Web サイトを起動してもらい、Codespaces で転送されたブラウザーを使って、更新されたユーザーインターフェイス (UI) を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに5段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能ができました。次はプルリクエスト (PR) を作成し、新しいコードをプロジェクトにマージします。

1. 既定のエージェントに変更の commit を依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR 作成時に既存の commit を push し、PR の URL を表示します。
3. Copilot CLI に表示された URL を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。
4. 変更されたファイルとチェックを確認します。
5. 準備ができたら **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って最初の変更をリリースしました。具体的には、次の作業を行いました。

- 機能ブランチで目的を絞った Copilot の会話を始めました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送したブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルタリングの Issue から始め、Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を実装します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
