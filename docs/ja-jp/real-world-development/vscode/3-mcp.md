---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルタリング機能を構築しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP server** を接続し、Copilot に実際のブラウザーを操作して機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot にツールを追加する方法を学びます。
- Playwright MCP server をワークスペースに追加します。
- Copilot に MCP server を使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外に接続できるとさらに強力になります。Copilot は Model Context Protocol (MCP) を使って外部のツールやサービスと通信します。これはエージェントが外部ツールと通信するための標準です。

![agent mode の内部構造と、コンテキスト、LLM、MCP server および VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部ツールやサービスと通信できます。MCP を使うことで、AI エージェントはリソースから最新情報を取得し、ツールを使ってユーザーに代わって操作できます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスを仲介する MCP server を通じてアクセスします。MCP server は、既存の API や NPM package などのローカルツールとの通信を管理します。MCP server ごとに、AI エージェントが使えるツールとリソースのセットが異なります。

よく使われる MCP server の例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する一連の API を利用できます。AI エージェントはリポジトリの作成、更新、Issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化を利用できます。AI エージェントは Web ページへの移動、フォームの入力、ボタンの選択などを行えます。

ほかにもさまざまなツールやリソースにアクセスする MCP server があります。GitHub の [MCP registry](https://github.com/mcp) を使うと、エコシステム内の server を検索してコントリビュートできます。

> [!CAUTION]
> セキュリティ上の影響を考慮し、MCP server はプロジェクト内のほかの依存関係と同じように扱ってください。使用前にソースコードと発行者を慎重に確認してください。信頼できる MCP server のみを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP server を設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` server があらかじめ設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot は Issue のバックログを読めました。[リモート GitHub MCP server][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP server を追加する

次に2つ目の server を追加します。[Playwright MCP server][playwright-mcp-server] を使うと、Copilot がブラウザーを操作して機能をテストできます。

1. `.vscode/mcp.json` で `github` と並ぶように `playwright` エントリを追加します。ファイルは次のようになります。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` server とは異なり、`playwright` は**ローカル** server です。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを付けると、画面を表示せずにブラウザーが動作します。デスクトップ画面がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright を使っているため、Playwright が必要とするブラウザーは通常すでにインストールされています。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行するよう依頼します。

## server を起動して信頼する

VS Code は MCP server を**必要なときに**起動します。Copilot が初めて server のツールを使うときに server が起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。これを選択して server を起動し、設定が正しいことを確認します。
2. VS Code が server を信頼するか確認したら、設定をレビューして信頼することを選択し、server を起動できるようにします。

手動での起動は任意です。省略した場合は、次の手順で Copilot が初めてツールを使うときに、VS Code が server を起動して信頼を確認します。

## フィルタリング機能をテストする

[Playwright MCP server][playwright-mcp-server] を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントにページを開き、移動し、フィルターを適用して結果を読み上げてもらい、確認内容を要約できます。会話を離れずに機能を確認できます。

Playwright MCP server はスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使って動作します。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などラベルが付いた構造化要素を参照します。そのため、簡単な機能確認がアクセシビリティの基本確認にもなります。

server を接続し、アプリを起動した状態で、構築したフィルタリング機能を試すよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server 経由でブラウザーを起動し、各手順を確認して結果を報告します。結果を Issue の受け入れ条件と照らし合わせます。問題があれば追加で質問するか、プルリクエストを作成する前にコードの修正を依頼してください。

> [!NOTE]
> このテストでは、アプリが `http://localhost:4321` で動作している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP server を初めて使うときは、ブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら `npx playwright install chromium` を実行して再試行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームでレビューできるようプルリクエスト (PR) を作成します。最初に `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンで commit message を生成します。

   ![Source Control パネルで変更を表示するスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選び、ブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、PR の作成方法はいくつかあります。GitHub Copilot を使っているので、PR の作成を依頼しましょう。関連する Issue を見つけ、Issue と関連付けた PR を作成できます。

1. Copilot Chat パネルで **New Chat** を選び、新しいセッションを開始します。
2. GitHub ツールを Copilot が使えるよう、エージェント一覧から **Agent** を選びます。
3. Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の取得や操作を許可するため、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときは、GitHub での認証を求められる場合があります。画面の指示に従って許可します。
5. Copilot が Issue を検索して適切なものを見つけ、PR を作成します。
6. Copilot が生成したリンクを選び、プルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP server が Copilot にツールを追加する方法を学びました。
- Playwright MCP server をワークスペースに追加しました。
- ブラウザーでフィルタリング機能を手動テストするために利用しました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP server を使って機能を**テスト**しました。MCP は Copilot を拡張する方法の1つにすぎません。次は、アクセシビリティレビューなどの作業を効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
