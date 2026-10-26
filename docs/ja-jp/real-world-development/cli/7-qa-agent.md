---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。次は、それらを明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールをどのように組み合わせるかを確認します。
- 再利用できる品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリングの Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を開く前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと、この QA プロセスを調整し、再利用できるレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown のプロファイルで定義する、Copilot の専門化された役割です。プロファイルに目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明し、`quality-checks` スキルは繰り返し可能なチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルは、それらを使って要件を評価し、結果を報告する方法を Copilot に伝えます。これらの仕組みを置き換えるものではなく、新しい会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用できる QA プロファイルを作成するよう Copilot に依頼します。プロファイルには、QA が行うチェックと守るべき範囲を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の手順と制約をプロファイルに記述したことを確認します。検証だけを依頼したのに、範囲が広すぎたり不完全だったりするエージェントが機能を変更するのを防げます。

1. `/diff` を入力して `.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須で、`name` は任意ですが、指定するとエージェント名が分かりやすくなります。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示すこと、実装コードを変更する前に確認を求めること、変更を commit したりプルリクエストを作成したりしないことを確認します。
5. これらの役割や制約が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue を使って QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルタリングの会話を再開して QA を選択し、Issue と計画時の決定事項を使えるようにします。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェント一覧に表示されないことがあります。それでもエージェントは存在し、上記のコマンドで有効にできます。

2. 次のプロンプトで機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 完了したら、レポートを最後まで読みます。

## まとめと次のステップ

再利用できる専門家の役割をワークフローに追加し、その作業を確認しました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールをどのように組み合わせるかを確認しました。
- 再利用できる品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの Issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、それらを[機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
