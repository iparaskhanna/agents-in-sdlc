---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加のため、GitHub Copilot と複数のタスクに取り組みました。コードへのドキュメントの追加、デザインチームが改善を重ねられる関連ゲーム機能の作成、高コントラストモードとライトモードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善してから、チームのレビューを有効にしましょう。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。AI が生成したコードをレビューし、通常の DevOps プロセスに従います。チームのコードレビューを有効にする前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監視されずにタスクを実行するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みアクセスは作業用ブランチに限られます。
- cloud agent は GitHub Actions 内で、別の一時的な環境を作成して作業します。
- GitHub Actions のワークフローは、人間が承認するまで実行されません。
- MCP server を含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR) を開き、コードにドキュメントを追加する作業を確認します。github.com の標準 PR インターフェイスを使います。

> [!NOTE]
> PR を開いたとき、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot の外部リソースへのアクセスは既定で制限されており、外部 MCP server への呼び出しも含まれるため、これは想定内です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. 新しいブラウザータブで github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれています。その場合は作業が完了するまで待ちます。数分かかる場合があります。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc の doc comment など、更新されたコードを確認します。変更内容は作業によって異なります。演習 2 の開始時に確認したカスタム指示ファイルから、TSDoc の doc comment、TypeScript の規約、コメントヘッダーが適用されたことを確認します。
6. レビューが終わり問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro app に対するプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot と PR で作業するときは、指示を一方的に送るだけではありません。チームメンバーと同じように、PR のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確率的であるため、具体的なプロンプトを1つに限定する必要はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc の doc comment を追加する。
- Astro app の構成を説明する README を作成する。

1. 上記の例などを使って、コードのドキュメントへの変更をコメントで依頼します。ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために Copilot が新しいセッションを開始することが分かります。
3. **Back to pull request** を選択して PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、新しい commit が PR に追加されます。
5. **Files changed** タブを選択して変更を確認します。

納得できるまで繰り返し Copilot に変更を依頼できます。準備ができたら、PR を draft からレビュー可能な状態に変更し、`main` にマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** または同様のタイトルの PR を選択します。
4. **Files changed** タブを選び、生成されたコードをレビューします。
5. レビューが終わり問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示があることを確認します。
7. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェックセクションでワークフローが実行されます。問題がなければ、単一の Astro app に対するプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動で試すこともできます。Codespace でターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot は関連ゲーム機能を実装しました。これまでと同じように、Copilot cloud agent にコメントを追加して変更を依頼できます。たとえば、表示する関連ゲームの数や、コメントヘッダーと TSDoc の doc comment の追加を頼めます。この Issue はカスタム指示を更新する前に割り当てたため、後者の変更が必要になる場合があります。**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能を確認します。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 の mission control から依頼したライトモードの両方が含まれています。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** または同様のタイトルの PR を選びます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれています。完了するまで数分待ちます。

4. **Files changed** タブを選択してコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保持する local storage の使用方法。
   - 高コントラストモードとライトモードの CSS またはスタイル変更。
   - アクセシビリティ属性 (ARIA label、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。
6. レビューが終わり問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示があることを確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェックセクションでワークフローが実行されます。問題がなければ、単一の Astro app に対するプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動で試すこともできます。Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリケーションを起動してブラウザーで高コントラストモードとライトモードを試し、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿ってこれらの機能を実装するよう Copilot を導いたことを確認します。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に変更を依頼できます。

## オプションの演習 — さらに作業を委任する

cloud agent に実際のバックログ項目を渡して、別の作業に取り組む方法を身に付けましょう。リポジトリに Issue をいくつか登録して Copilot に割り当てます。たとえば、次のような内容があります。

- ゲームの詳細ページに支援者の関心フォームを作成する。
- ゲーム一覧ページにページネーションを追加する。
- データアクセス helper に入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認**し、後で cloud agent の出力にどう影響するか確認できるようにしました。
- **Copilot cloud agent に Issue を割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成、使用**し、高コントラストモードとライトモードを追加しました。
- **Copilot agents page を mission control として使用**し、進行中にアクセシビリティのセッションを監視して誘導しました。
- **cloud agent のプルリクエストをレビューして改善**し、`@copilot` に変更を依頼してワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスを使うと、学んだ内容をさらに発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP を使います。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルから2つの変更をレビューしてリリースします。

自分のリポジトリでは、次のことを試してみてください。

- リファクタリングやテスト範囲の Issue を cloud agent に割り当てる。
- 別の専門分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
