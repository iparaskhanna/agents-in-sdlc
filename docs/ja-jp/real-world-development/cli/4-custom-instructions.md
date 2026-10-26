---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認してドキュメント標準を追加し、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを行う必要がある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]には、どのようなコードが必要かだけでなく、どのような構成にするかも記述します。フィルタリング機能を構築したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位で適用される指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントにはコードを言い換えるだけではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。該当する場合は、注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の仕様を記述し、関連コードを変更したときはコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿った作業に必要な情報を得られるようにします。

## 指示ファイル

カスタム指示では、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくします。指示を使うと、関連性の高い提案やコードスニペットが得られます。好みのコーディング規約、ライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体または特定のタスクやファイル種別を対象にできます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリ内の**すべての**リクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係するプロジェクトレベルのコンテキストを記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記述します。

> [!NOTE]
> ほかの形式の指示とそのサポート状況は、環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポート情報][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

変更を加える前に、スタータープロジェクトに含まれる指示ファイルを確認します。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの説明とコーディングのガイダンスを確認します。この指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データレイヤー、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob で、指示が適用されるファイルを指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな指示セットを小さく再利用できるファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。中心となる `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** セクションを探します (35行目付近)。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、フィルタリングの会話を終了して再開すると、機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルタリングの会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたらフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを読み込む方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス単位で適用される指示ファイルがエージェントに適用される仕組みを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用できる quality-checks スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート情報][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
