---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここ数回の演習では、Copilot cloud agent にユーザー体験の改善や機能追加に関する3つのタスクを依頼しました。cloud agent は非同期で自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するためのツールがいくつかあります。この mission control では、開いているプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。エージェントが行った操作を確認し、進行中のセッションを誘導できます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションを誘導し、機能の追加を依頼します。

## シナリオ

高コントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加することにしました。サイトのスタイルと切り替え機能を更新しているため、ライトモードも含めるのが自然です。高コントラストモードに加えてライトモードを追加するよう、エージェントを誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents page](https://github.com/copilot/agents) に移動します。
2. メインペインと左ペインの両方にあるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新。
   - ゲームを変更する API の生成。
   - Web サイトへの高コントラストモードの追加。
3. 実行中のタスクを選び、Copilot が行った作業を確認します。次のような操作が表示されます。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP server をセットアップする。
   - タスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される正確な手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR ペインも確認します。ここから PR と変更されたファイルを確認し、さらに監視できます。

## cloud agent を誘導する

実行中のタスクを確認したので、高コントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. 高コントラストモードの追加に関するセッションを選択します。正確なタイトルは、Copilot が付けた名前や現在の作業状況によって異なります。

   ![mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セッションを数分間確認し、セットアップが終わって作業を始めたことを示すメッセージが表示されるまで待ちます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページで、Copilot が作業中のセッションを誘導するダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> キーを押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めることを確認します。

## Copilot に作業を続けてもらう

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信したときに進めていた手順が完了してから、新しい依頼を作業に取り込みます。

これにも数分かかります。その間に、ここまでに学んだ内容を振り返ってください。

## まとめと次のステップ

Copilot cloud agent のタスクを監視、誘導する中心的な場所である Copilot agents page を確認しました。mission control では、進行中と完了済みのタスクを追跡し、作業内容をレビューし、進行中のタスクの範囲を変更したりガイダンスを追加したりできます。

このレッスンでは、次のことを行いました。

- mission control とエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを改善します][next-lesson]。

## リソース

- [Copilot agents page][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
