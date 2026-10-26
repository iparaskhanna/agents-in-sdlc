---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業を終えると、人間の開発者がレビューできる draft PR を作成します。面倒な作業を任せることで開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次の内容を学びます。

- コード生成の環境をカスタマイズする方法。
- 操作を安全に実行する方法。
- 範囲を明確にした Issue の重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託業者がドキュメントを十分に追加しておらず、ほとんどない状態です。最初の対応として、アプリケーション内のエクスポートされたすべての関数に TSDoc の doc comment を追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。ゲームの詳細ページに、同じカテゴリの別のタイトルを関連ゲームとして表示し、閲覧を続けられるようにします。まだ仕上げは不要で、UX の受け入れテストに使える状態であれば十分です。この作業は現在ブロックされていますが、優先度の高い Issue がほかにあります。

こうしたタスクは優先度が下がりやすいため、Copilot cloud agent に割り当てるのに適しています。cloud agent は非同期で作業するため、開発者は別の作業に集中し、後から戻って成果を確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業します。[GitHub Issue を Copilot に割り当てる][assign-issue]など、複数の方法でタスクを依頼できます。割り当てると、Copilot は進捗を追跡する draft のプルリクエストを作成し、環境を準備してタスクに取り組みます。作業中や完了後に Copilot のセッションを確認できます。解決案のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot を使っても考える作業を省略できるわけではありません。一見簡単な操作にも、詳しく見ると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲を適切に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして捉え、段階的に進め、学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習で、カスタム指示が Copilot の作業を導く方法を学びました。すでにカスタム指示のハンズオンを完了していれば `.github/copilot-instructions.md` の動作を確認しています。まだの場合は、この機会に簡単に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリにある指示ファイルを読み取り専用で確認すると、後で影響を見つけやすくなります。

実行中の Codespace があればそこで、なければリポジトリの GitHub Web UI で次のファイルを開きます。

- `.github/copilot-instructions.md` — TypeScript の規約と TSDoc の doc comment に関する**Code standards** セクションを確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、生成されたプルリクエストで TSDoc の doc comment、TypeScript の規約、コメントヘッダーを確認します。これらは指示ファイルの内容に基づいています。後の演習で PR をレビューするときに、もう一度確認します。

## Copilot cloud agent の開発環境をセットアップする

誰が作業する場合でも、コードを作成するときは特定の環境とセットアップが必要になることがあります。Copilot はソフトウェアエンジニアと同様に作業するため、タスクを割り当てる場合にも環境が必要です。

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、必要な開発ツールと依存関係を使えるよう、Copilot が作業を始める前に処理を実行できます。このラボでは、学習を進めやすくするため、あらかじめ設定されています。単一の Astro app に必要な Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを実行した SQLite データベースが利用可能になります。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルに似ていますが、次の重要な点があります。

- `copilot-setup-steps` という1つのジョブがあります。Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動で実行できます。Copilot に実行を任せる前に、正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントが重要なのは誰もが知っていますが、情報が古かったり、まったく存在しなかったりするプロジェクトも少なくありません。放置されると生産性が下がり、コードベースの保守や新しい開発者の参加が難しくなります。Copilot はドキュメントの作成を得意とするため、cloud agent に依頼するのに適した作業です。バックグラウンドで必要なドキュメントを生成してもらい、後の演習で成果を確認します。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

   ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前です。一覧に見つからない場合は、しばらく待ってブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は先ほど説明したワークフローを実行して環境を準備し、プロジェクトを分析して、作業の進め方を決めます。

13. todo リストを確認し、実行予定の作業を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が見えない場合は、ブラウザーを更新してください。

16. セッションをスクロールし、Copilot の作業を確認します。コードを調べて状態を理解し、計画を検討して決定し、コードを作成する様子が分かります。

完了まで数分かかることがあります。Copilot cloud agent の主な目的は、非同期で作業し、その間にほかのタスクに集中できるようにすることです。別のタスクも Copilot cloud agent に割り当てて、アプリケーションへの機能追加を進めましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の1つは、作業を分担できることです。デザインチーム向けに関連ゲームを追加する作業が短時間で済む場合でも、その間に別のタスクを進められます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

しばらくすると、Issue の最初のコメントに 👀 が表示されます。Copilot が作業を始めた合図です。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアのように作業するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] について学びました。Issue を Copilot に割り当てると、非同期で作業を依頼できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次の内容を学びました。

- コード生成の環境をカスタマイズする方法。
- 操作を安全に実行する方法。
- 範囲を明確にした Issue の重要性。
- Issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent では MCP server][cloud-agent-mcp] も使用でき、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
