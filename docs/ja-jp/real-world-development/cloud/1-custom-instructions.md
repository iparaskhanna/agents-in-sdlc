---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく、進め方も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位で適用される指示ファイルを通じて、プロジェクトのコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前と変更後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue を基に非同期で作業するため、リアルタイムで影響を並べて確認しにくいからです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

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

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。多くの chat や CLI リクエストに関係する、技術スタック、プロジェクトの概要、ベストプラクティスなどの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成します。TypeScript や Astro などの言語、UI コンポーネントや単体テストの追加などのタスクにガイドラインを提供できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat によるコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと、`applyTo` frontmatter のある `*.instructions.md` ファイルの両方を使います。
>
> Copilot は [ほかの標準に基づく指示ファイル][custom-instructions-support]もサポートしており、AGENTS.md や CLAUDE.md ファイルも利用できます。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。サンプルプロジェクトの例から、代表的な方法を確認できます。

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記述します。
- `*.instructions.md` は、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種別や特定のタスク向けの詳細な指示に使います。
- 明確な自然言語を使い、望ましいコードと望ましくないコードの例を示します。

指示ファイルを作る方法も AI を使う方法も、唯一の正解はありません。試行錯誤し、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルのセットが必要です。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作ってもらうこともできます。利用する環境によって方法は異なり、たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。該当するレッスンで詳しく説明します。
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
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭にある `applyTo` フィールドを確認します。リポジトリルートからの相対パスで glob を設定し、対象ファイルを指定します。この例では、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトで単体テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内のファイルへのリンクがあります。大きな指示セットを小さく再利用できる単位に分け、コード生成時に Copilot が参考にする例を示せます。(記載されたパスはリポジトリルートではなく、この指示ファイルからの相対パスです。)

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc の doc comment とファイルコメントのヘッダーを追加するルールを設定します。

## リポジトリの新しい標準を追加する

この演習で編集するのは、コード内のドキュメントを TSDoc の doc comment とファイルレベルのコメントヘッダーとして記述する、プロジェクト全体のルールです。後の演習で cloud agent が Issue に取り組むときにこのルールを読み込みます。

編集を始める前に作業ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、ここでは変更を整理して commit します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案の質を高められるよう、リポジトリの標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します (27行目付近)。コーディング標準は記載されていますが、コード内のドキュメントに関するルールがありません。そのため、生成された helper には doc comment がありませんでした。
3. 既存の標準の直後に次の Markdown 行を追加し、ファイルコメントのヘッダーと TSDoc の doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したとおり、指示ファイルは全体的なガイダンスを記載するリポジトリレベル (`.github/copilot-instructions.md`) にも、言語、ファイル種別、タスクに対応する `*.instructions.md` にも作成できます。今回追加する doc comment ルールのような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更を commit、push してマージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。そのため、cloud agent が新しい指示を使えるように、変更を `main` にマージする必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue に取り組む際、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは、`main` に直接作業してもかまいません。その場合はブランチの手順を省略し、`main` に直接 commit してください。ここでブランチを使うのは、実際のプロジェクトに近い手順を体験するためです。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` に新しい標準を追加しました。このハーネスの後半で cloud agent が作成するプルリクエストから、その標準が使われた様子を確認します。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot をカスタマイズする指示ファイル][instruction-files]
- [カスタム指示の作成に関するベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示をよりよく書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
