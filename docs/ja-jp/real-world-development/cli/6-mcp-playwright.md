---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く以外にも、データや外部サービスを扱い、追加の自動化を Copilot が利用できるようにする必要があります。そこで MCP server が役立ちます。MCP server を使うと、CLI に組み込まれた機能だけでなく、ほかのツールやサービスも Copilot から利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを理解します。
- まだ利用できない場合は Playwright MCP server を追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

単体テストとエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot がユーザーのように Web サイトを使えるようにし、変更が期待どおりに動作することをより確実にします。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントが外部ツールやサービスと通信できます。MCP によって、AI エージェントはリアルタイムで最新情報にアクセスし、ユーザーに代わって操作できます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスを仲介する MCP server を通じてアクセスします。MCP server ごとに、AI エージェントが使えるツールとリソースのセットが異なります。

よく使われる MCP server の例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化を利用できます。

ほかにも多くの MCP server があります。GitHub の [MCP registry][mcp-registry] では、エコシステムの検索やコントリビューションができます。

> [!CAUTION]
> MCP server は、プロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードと発行者を確認し、セキュリティ上の影響を検討してください。

## Playwright MCP server を追加する

Playwright MCP server がまだ利用できない場合は、Copilot がユーザーのように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに設定されている場合は、セットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP server の追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` を入力し、<kbd>Tab</kbd> キーを押します。
4. <kbd>Enter</kbd> キーを押して **STDIO** の server type を確認し、さらに <kbd>Tab</kbd> キーを押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して新しい MCP server を保存します。
7. <kbd>Esc</kbd> キーを押して MCP ダイアログを終了します。

## Playwright で機能を試すよう Copilot に依頼する

前のレッスンでは、機能が期待どおりに動作することを手動で確認しました。今度は Copilot が利用できる Playwright MCP server を使って同じ確認を行います。

1. Copilot に Playwright MCP server を使って機能を検証するよう、次のプロンプトで依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自分で MCP server を見つけて使うため、明示的に指定する必要はありません。ただし、使用してほしい server が分かっている場合は、名前を伝えると結果が安定し、トークンも節約できます。

2. Copilot がブラウザーで機能を確認する手順を示す様子を見ます。
3. レポートを読み、すべてが期待どおりに動作するか確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP server を使って、実際のブラウザーで機能を確認しました。次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びました。
- まだ利用できない場合は Playwright MCP server を追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を試しました。

次は、スキルとブラウザーツールを専門家の役割にまとめる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP server を追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
