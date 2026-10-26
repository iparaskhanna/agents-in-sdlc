---
title: "演習 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、登録済みのフィルタリング issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールと連携できます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずはツールをインストールしましょう。使い慣れたツールでインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングに関する issue を見つける。

## シナリオ

チームでは、増え続けるバックログへの対応に AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に作業するターミナルで AI エージェントを活用できます。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、device code を使った認証手順に従い、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 基本操作を確認する

通常のシェル プロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語でエージェントに指示し、スラッシュ コマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みバージョンで使えるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 簡単な質問をして、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答の前にリポジトリを調べる様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. Copilot にフィルタリング用の issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。簡単な変更を行った後、この issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使用します。worktree は自動的に作成されません。変更ごとに、作業用のブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、設定済みの権限外でツールを使う前に通常は確認を求めます。このワークショップでは、`--yolo` を指定して再起動します。これにより、Codespace 内の承認プロンプトがなくなり、演習に集中できます。

> [!CAUTION]
> `--yolo` はすべての権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や仮想マシンなどの隔離された環境でのみ使用し、日常の開発で使う既定値としてエイリアスを設定しないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` を指定すると、後のレッスンで issue やプル リクエストを操作するための読み取り/書き込み GitHub MCP ツールが有効になります。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開またはマージする前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう一度簡単な質問をして会話が機能することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンでは、指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って、フィルタリングに関する issue を見つけました。

次は、Copilot CLI を使ってゲーム カードに星評価を表示する最初の変更に取り組みます。[次のレッスン][next-lesson]に進んでください。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
