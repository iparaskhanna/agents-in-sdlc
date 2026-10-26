---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズしてフィルタリングの検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く以外にも、必要な作業があります。コードが手動で動作することを確認し、指示ファイルを使って標準に沿うようにしました。次はテスト、lint、継続的インテグレーション (CI) のそのほかの作業を扱います。

このような作業には、**エージェントスキル**が適しています。スキルを使うと、Copilot がこれらの操作を適切に実行する方法を学べます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの指示、実行可能なスクリプト、関連リソースをまとめ、エージェントが必要に応じて読み込めるようにします。基本的には、スキル名のフォルダーにある `SKILL.md` が中心です。Markdown には、スキルの名前と説明を定義する frontmatter、概要、呼び出すタイミングのガイダンスを記述します。フォルダーにスクリプトやその他のリソースを追加することもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトの既存コマンドを使うため、`SKILL.md` のみで構成されています。

スキルをプロジェクトの `.github/skills` フォルダーに配置すると、チーム内で共有、再利用できます。ユーザーのスキルフォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェック用に作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces のエディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。`description` は Copilot がこのスキルをいつ呼び出すか判断する助けになります。
3. 指示を読み、テストと lint の手順をどのように案内するか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot にスキルの3つのチェックを実行してもらいます。

1. Interactive モードでフィルタリングの会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した情報を示す、よりよいレポートを作成しましょう。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに編集済みのスキルを再読み込みできます。更新を確認しましょう。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前と同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使いました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能の PR に含めます。次は、[Playwright MCP server][next-lesson] を使い、Copilot がサイトを直接操作できるようにします。

## スキルの例

次のコミュニティの例は参考資料です。追加課題ではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱のエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
