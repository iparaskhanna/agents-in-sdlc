---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにします。Copilot とコンテキストを共有する方法はいくつかあります。コード生成のガイダンスを伝える[指示ファイル][instruction-files]が特に役立ちます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位で適用される指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- *現在の*指示を使ってフィルタリングの最初のデータスライス (publishers helper) を作成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に従う様子を確認します。
- 指示の更新とフィルタリングの土台を `main` に commit し、次の演習で Copilot が更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが設定した標準から外れることがあります。Copilot は確率的に動作します。この演習では、指示の更新後に動作がどう変わるかを確認します。出力を1文字ずつ一致させる必要はありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには常に単体テストが必要です。
- UI はダークモードにし、モダンな印象にします。
- コードのドキュメントは TSDoc の doc comment として記述します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業できる情報を渡します。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。好みのコーディング規約、ライブラリ、コードに含めるコメントなどを指定すると、関連性の高い提案やコードスニペットを得られます。リポジトリ全体または特定のファイル種別に適用できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。技術スタック、構築するものの概要、ベストプラクティスなど、多くの chat や CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成します。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などのタスクにガイドラインを提供できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat によるコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと `applyTo` frontmatter のある `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [ほかの標準に基づく指示ファイル][custom-instructions-support]もサポートしており、AGENTS.md や CLAUDE.md ファイルも利用できます。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することは、この演習の範囲外です。サンプルプロジェクトに含まれる例から、代表的な方法を確認できます。

- `copilot-instructions.md` には、構築するもの、プロジェクト構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` は、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種別や特定のタスク向けの指示に使います。
- 明確な自然言語を使い、望ましいコードと望ましくないコードの例を示します。

指示ファイルの作成方法も AI の使い方も、唯一の正解はありません。試行錯誤し、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルのセットが必要です。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作ってもらうこともできます。利用する環境によって方法は異なり、たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。該当するレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みます。中心となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭にある `applyTo` フィールドを確認します。リポジトリルートからの相対パスで glob を指定し、指示の適用対象を決めます。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトで単体テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの指示ファイルやプロジェクト内のファイルへのリンクがあります。大きな指示セットを小さく再利用できる単位に分け、コード生成時の参考例を Copilot に示せます。(ここに記載されたパスはリポジトリルートではなく、この指示ファイルからの相対パスです。)

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc の doc comment とファイルコメントのヘッダーを追加するルールを設定します。

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習ごとに異なるため、必要な箇所で説明します。

カスタム指示の効果を確認するため、まず現在の指示を使ってプロンプトを送ります。その後で指示を更新し、同じプロンプトをもう一度送信して違いを確認します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. `src/lib/publishers.ts` を開き、helper を置く場所を Copilot に知らせます。
3. Chat ビューのエージェント一覧から **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。helper とそのテストなど、複数のファイルを変更する場合があります。
6. 提案された helper が、最初の引数として `db` client を受け取り、publishers の型付き配列を返す型付き関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc の doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。指示の更新後に一貫性が向上することが重要です。

## リポジトリの新しい標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案の質を高めるため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します (27行目付近)。標準は記載されていますが、コード内ドキュメントのルールはありません。そのため、生成された helper に doc comment がありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。ファイルコメントのヘッダーと TSDoc の doc comment を追加するルールです。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したとおり、指示ファイルはリポジトリ全体のガイダンスを記載する `.github/copilot-instructions.md` にも、言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。今回追加した doc comment ルールのようにプロジェクト全体に適用する標準には、リポジトリレベルのファイルが適しています。

## プロンプトをもう一度実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルを対象にするようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot が新しい標準に従うよう誘導し、次の演習で利用する実際のコードに適用できました。

## 指示とブランチを commit、push する

指示ファイルもリポジトリ内のほかのファイルと同じように、ソース管理で管理します。変更を commit してリポジトリに push しましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示と helper の変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と helper の土台を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、バックログのドキュメント標準に関する Issue と関連付けたプルリクエストを作成します。手動で作成することもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。Issue を検索し、Issue を閉じる PR を作成して、マージしてもらいましょう。

> [!NOTE]
> Copilot に GitHub の操作を依頼するのは初めてです。プロジェクトのテンプレートでは GitHub のツールがワークスペースに設定されているため、追加の設定は不要です。初めて Copilot がツールを使用するとき、VS Code から GitHub へのサインインを求められる場合があります。プロンプトに従って許可してください。後の演習で、この接続に使われる Model Context Protocol について学びます。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント一覧で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新とフィルタリング用 publishers helper の土台の両方を PR に記載します。

   ```text
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認し、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```text
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作とシェルコマンドの実行を許可するよう求められたら、コマンドを確認し、適切な場合に **Allow** を選択します。

GitHub Copilot の助けを借りて、プルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示を使ってコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加しました。
- 同じプロンプトをもう一度実行し、提案されたコードが新しい標準に従う様子を確認しました。
- 指示の更新と helper の土台を `main` に commit し、次の演習で利用できるようにしました。

次は、[agent mode][next-lesson] を使い、コードベース全体に新機能を追加する際に指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示の作成に関するベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示をよりよく書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
