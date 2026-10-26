---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔に進めるため、フィルタリングと品質確認の作業を同じ会話とブランチで行い、1つの PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びます。
- 機能の変更全体と検証の証拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージの承認を開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージ可能にするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に対処します。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合の rebase を行い、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその作業を任せられますが、有効にする前に変更をレビューし、マージしてよい状態であることを確認してください。

## Agent Merge で PR を管理する

コードが揃ったので、まとめてレビューして PR を作成し、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> キーを押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、必要な権限を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている通常のレビュー済みマージを手動で行います。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストの処理など、開発プロセスのいくつかを自動化しました。次のことを行いました。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びました。
- 機能の変更全体と検証の証拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に便利な[Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
