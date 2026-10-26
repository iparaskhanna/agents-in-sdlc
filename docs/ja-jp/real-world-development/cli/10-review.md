---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用できるカスタマイズ、参考リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルタリング Issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの確認内容を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR で Agent Merge の使用を承認しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップでは、更新済みの `main` から作成した別々のブランチで、2つの PR の節目を経験します。

1. **星評価:** 既存の `starRating` と、未評価であることを明示する表示をゲームカードに追加します。
2. **フィルタリングと品質確認のワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、これらの作業を1つの PR にまとめました。

## さまざまな方法で検証する

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー確認など、複数の方法でコードをチェックしました。`quality-checks` スキルは、単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA ではそれらの結果に加えて、要件とテスト範囲をレビューしました。

追加したテストは実際の不足を補う必要があります。QA の実行で新しいテストが不要と判断されても、適切な場合があります。マージを承認する前にコードと検証の証拠をレビューし、変更があった場合は影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールは、作業結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP server を設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し可能なタスクを記述し、カスタムエージェントは専門家の役割を定義し、MCP server は外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、目的を絞った変更を自分で確認しながら進めるには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を作るのか、なぜ必要なのか、どのように進めるのかを明確に伝えると、結果が大きく変わります。

## さらに試す

中心的なワークフローを学びました。次の CLI 機能も試してみてください。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して実行します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールも、使い続けることで活用方法が身に付きます。本番コード、趣味のコード、長年考えていた小さなアプリなどで使ってみてください。学んだことをチームで共有し、チームからも学びましょう。そして、ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境での体験と比べるには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
