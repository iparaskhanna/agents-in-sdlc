---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専門化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業をより効果的に進めるための指示、ベストプラクティス、コーディングパターン、専門知識を Copilot に伝えます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディング方法に従うセキュリティエージェント、一貫したテストパターンを保つテストエージェントなど、チームの専門知識を再利用できるエージェントとして定義できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはユーザーの `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには `name` と `description` を含む YAML frontmatter があり、その後にエージェントの動作、専門知識、指示を記述する Markdown プロンプトが続きます。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、共通するところがあります。どちらも主に Markdown ファイルで定義し、AI に操作方法を伝えます。簡単に区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには専用のコンテキストウィンドウがあり、作業の一環としてスキルや他のエージェントを調整できます。このラボのアクセシビリティ用カスタムエージェントは、アクセシビリティガイドラインに照らしてサイトをレビューし、更新します。作業中に、プルリクエスト用のワークフロースキルやテストの実行、管理に使うスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの用途と同じように、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使い、次のことを確認します。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディングプラットフォームを使えるように取り組んでいます。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不十分で、現在のダークテーマが読みにくいという声がありました。デザインチームは、このアクセシビリティの問題に対応するため、ユーザーが切り替えられる高コントラストモードを実装したいと考えています。

アクセシビリティは重要なため、カスタムエージェントを使って機能をすばやく生成します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。内容を確認し、Copilot の動作をどのように導くかを理解しましょう。

Codespace に戻り、アクセシビリティ用カスタムエージェントファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシビリティに配慮した Web サイトのコードを生成する際の中心的な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や誤り。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを管理する中心的な場所です。ここから Copilot cloud agent にタスクを割り当て、進捗を監視し、追加のガイダンスを伝えられます。まず、高コントラストモードの作成を Copilot に依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントの割り当て画面で、Accessibility が選択されているスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、特定のタスクや分野に合わせた AI アシスタントです。チームの専門知識や標準を再利用できるエージェントにまとめ、Copilot の作業を適切に導けます。

このレッスンでは、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- タスクをカスタムエージェントに割り当てました。

Copilot が高コントラストモードを実装している間に、[mission control からエージェントのセッションを監視して誘導します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
