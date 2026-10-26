---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。まとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔に進めるため、フィルタリングと品質確認のワークフロー全体を1つのセッションとブランチで行い、すべての作業をこの PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルを自動化する仕組みを学びます。
- 機能の PR 全体と検証の証拠を確認します。
- レビュー後にのみ Agent Merge を承認し、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローを通じて、Copilot を使い機能を計画、実装、検証しました。Tailspin Toys では、マージの承認を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** を使うと、GitHub Copilot app でプルリクエストをマージするまでの残りの作業を自動化できます。有効にすると、アプリのセッションが PR を読み込み、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じた rebase など、マージを妨げている問題に対処します。GitHub で許可され次第マージします。バックグラウンドで動作し、アプリを再起動しても継続し、PR がマージされると自動的に停止します。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできますが、コード編集やマージの権限を与えるには明示的な承認が必要です。マージ権限を与える前に、許可される操作と作業を確認してください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了したので、Agent Merge に PR の作業を任せます。

1. エージェントピッカーで **Default agent** を選びます。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選びます。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンから選ぶだけでは開始されません。

Agent Merge が開始されると、次の作業を行います。

- タイトルと説明を含むプルリクエストを作成します。
- Issue からセッションを始めた場合は、説明の本文で関連する Issue を参照します。
- 必要に応じて rebase し、ターゲットブランチとのマージ競合に対処します。
- CI を監視し、すべてのチェックが成功するようにします。
- 他の開発者や Copilot code review からのフィードバックがないか PR を監視し、コメントを解決する更新を行います。
- すべてが成功したら、任意で PR を自動的にマージします。

すべてのチェックが成功したときに Agent Merge が PR もマージするように設定します。

5. マージ権限を与える前に、PR の差分と検証の証拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. 次のように明示的にエージェントにマージを依頼します。

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 進捗を確認し、報告された阻害要因を解決します。続行前に PR の状態が **Merged** であることを確認します。マージ権限が有効になったことやエージェントが実行中であることだけでは、マージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge はリポジトリの保護や権限不足を回避しません。続行する前に、それらの阻害要因を解消してください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の部分を自動化しました。次のことを行いました。

- Agent Merge とは何か、マージのライフサイクルを自動化する仕組みを学びました。
- 機能の PR 全体と検証の証拠を確認しました。
- レビュー後にのみ Agent Merge を承認し、PR がマージされたことを確認しました。

次は、[既存の Canvas を使ってトリアージ Canvas を作成し][next-lesson]、エージェントと一緒に作業を確認、計画、視覚化する方法を試します。

## リソース

- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
