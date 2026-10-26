---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専門化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業を効果的に進めるための指示、ベストプラクティス、コーディングパターン、専門知識を Copilot に伝えます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディング方法に従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントなど、チームの専門知識を再利用可能なエージェントとして定義できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはユーザーの `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには `name` と `description` を含む YAML frontmatter があり、その後にエージェントの動作、専門知識、指示を記述する Markdown プロンプトが続きます。

### カスタムエージェントとエージェントスキルの比較

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に操作方法を伝えます。簡単に区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一環としてスキルや他のエージェントを調整できます。このラボのアクセシビリティ用カスタムエージェントは、アクセシビリティガイドラインに照らしてサイトをレビューして更新します。作業中にプルリクエスト用のワークフロースキルやテストの実行、管理に使うスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの用途と同じように、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディングプラットフォームを使えるように取り組んでいます。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不十分で、現在のダークテーマが読みにくいという声がありました。デザインチームは、ユーザーが切り替えられる高コントラストモードを実装したいと考えています。

アクセシビリティは重要であるため、カスタムエージェントを使って機能をすばやく生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、高コントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用カスタムエージェントはすでに作成されています。内容を確認し、Copilot の作業をどう導くか理解しましょう。

Codespace に戻り、ターミナルを開いて、フィルタリングの PR を演習 3 の作業と分けるため、`main` からアクセシビリティ用のブランチを作成して切り替えます。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティに配慮した Web サイトのコードを生成する際の中心的な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や誤り。

## Copilot Chat でカスタムエージェントを使う

VS Code は `.github/agents` にあるカスタムエージェントを、Copilot Chat ビューの下部にあるエージェント一覧に表示します。カスタムエージェントを選ぶと、その指示とツールに合わせたチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習ごとに異なるため、必要な箇所で説明します。

1. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。選択されていない場合は選びます。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント一覧を選択します。既定では、現在のエージェント (通常は **default**) が表示されます。
3. 利用できるエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べて変更を提案し、プロジェクトのファイルを編集します。編集内容は、ファイルパスと差分とともにチャット内に表示されます。
6. 編集が加わると **Files changed** インジケーターが更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この処理には数分かかります。Copilot は Astro コンポーネント、CSS、関連するテストなどの既存ファイルを変更します。

次の演習で、この作業を監視しながら誘導します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、特定のタスクや分野に合わせた AI アシスタントです。チームの専門知識と標準を再利用できるエージェントにまとめ、Copilot の作業を導けます。

この演習では、次のことを確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat の agent mode でカスタムエージェントを使う方法。

次は、[エージェントの作業を監視して誘導します][next-lesson]。変更を確認しながら、同じセッションにライトモードの切り替えも追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP server で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
