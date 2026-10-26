---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイルを変更し、テストを実行する必要があることがよくあります。開発者は通常、必要なファイルを探し、変更してテストを実行し、問題をデバッグし、変更し忘れたファイルを見つけて、さらに修正します。作業は続きます。

そこで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は IDE 内で自律的に動作します。開発者と同じように、既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って Web サイトに機能を追加する方法を見ていきましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる使い方をします。agent mode は IDE 内でタスクを実行し、すばやいフィードバックと対話を可能にします。cloud agent はチームのメンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行う方法。
- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリと発行者で絞り込めるようにします。前の演習で publishers helper を追加しました。次は Copilot Agent Mode を使って、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認して現在の機能を把握します。

この Web サイトは、開発者向けテーマのボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。Astro app はビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースに直接問い合わせます。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用できるデータアクセス helper は `src/lib/` に置き、対話機能には標準 DOM API を使う、小さく範囲を限定した Astro `<script>` を使います。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えて `main` にいることを確認します。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して Codespace 内に新しいターミナルを開きます。
3. フィルタリングの作業用にブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

Astro 開発サーバーが起動すると、次のような URL のバナーが表示されます。

```bash
🚀 Tailspin Toys is ready!
   Astro server: http://localhost:4321
   Press Ctrl-C to stop.
```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro server のアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。

> [!NOTE]
> Codespace ターミナルから localhost URL のリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Codespace で実行中の Web server へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習ごとに異なるため、必要な箇所で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善します。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されます。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェント一覧から **Agent** を選びます。**Agent** agent は自律的に計画を立て、複数ファイルを変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使ったため、体験を揃えやすくする目的で手順に記載しています。LLM は確率的に動作するため、提案が手順と異なる場合がありますが、問題ありません。

> [!NOTE]
> 確率的な動作のため、Copilot は別の MCP コマンドを使うことがありますが、同じタスクを完了できます。

3. 次のプロンプトを Copilot に送信し、バックログの Issue を表示してもらいます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選び、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーにとって有用だと判断した順に、Issue の優先順位も付けています。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを提供する指示ファイルを確認します。Web サイトへの機能追加方法が記載されている [user interface (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの指示ファイルへのリンク。

> [!TIP]
> 指示ファイルからほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。内容を再利用し、複雑な指示を小さな単位に分け、参考例やテンプレートを示せます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルタリング機能を実装する

フィルタリングを完成させるには、少なくとも次の3つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルタリングロジックを追加または調整する。
- フィルタリング動作のテストを追加または更新する。
- ゲーム一覧ページにフィルタリング UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼します。

1. Copilot との現在の会話を続けるか、**New Chat** を選んで新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選ぶ画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データレイヤーの helper、UI、テストを見つけ、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイル変更、テスト実行などを行います。タスクとコードベースの複雑さによって数分かかる場合があります。作業中にコードエディターへ **Keep** と **Undo** が表示されることがあります。作業が終わるとすべての変更に **Keep** または **Undo** が表示されるため、進行中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選び、テストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したりテストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを行き来し、完了してエラーが見つからなくなるまで作業します。

   ![Copilot Agent Mode とのセッションが完了した画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot を含む AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を試します。
11. すべてが動作し、コードのレビューが終わったら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行う方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP server で機能をテストし][next-lesson]、プルリクエストを作成します。

### おまけの演習 — ページネーションを実装する

ゲーム一覧が増えたらページネーションが必要になります。この演習で学んだスキルを使って、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従う。
- ユーザーがページサイズを選べるようにするか、固定するかなど、ページネーションの方法を検討する。
- 希望どおりに実装してもらうため、プロンプトに必要なガイダンスを含める。
- 変更を依頼してコンテキストを提供するなど、GitHub Copilot との反復が必要になる場合がある。これは通常の使い方です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
