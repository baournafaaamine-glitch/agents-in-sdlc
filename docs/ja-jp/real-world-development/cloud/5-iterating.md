---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、コードのドキュメント追加、関連ゲーム機能の実装、ハイコントラストとライトモードを含むアクセシビリティ機能を Copilot に依頼しました。変更内容を確認し、必要なら Copilot にフィードバックを送ります。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。ドキュメント、関連ゲーム、アクセシビリティ機能の提案をレビューし、通常の開発プロセスを経てからチームのレビューを有効にします。

## GitHub Copilot cloud agent のセキュリティ

cloud agent は非同期に、直接の監視なしで作業するため、安全性を保つための制約があります。

- Copilot はリポジトリの読み取り権限を持ち、書き込みは作業用ブランチに限られます。
- cloud agent は GitHub Actions 内に一時的な環境を作成して実行されます。
- GitHub Actions ワークフローの実行には、人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加した最初の Copilot cloud agent のプルリクエスト (PR) を github.com の標準 PR 画面で確認します。

> [!NOTE]
> PR に Copilot のファイアウォールによるブロック警告が表示される場合があります。MCP サーバーへのアクセスなど、外部リソースへのアクセスは既定で制限されているためです。必要に応じて [cloud agent のファイアウォールを設定または無効化][agent-firewall]できます。

1. github.com のリポジトリを開きます。
2. **Pull Requests** を選択し、PR の一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

Copilot が作業中の場合、PR に **[WIP]** が付いています。完了するまで待ちます。

4. **Files changed** タブを選び、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc コメントなど、生成されたドキュメントを確認します。変更内容は実行ごとに異なります。Exercise 2 の指示ファイルから適用される TSDoc、TypeScript 規約、コメントヘッダーにも注目します。
6. **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローが実行されることを確認します。単一 Astro アプリのプロジェクトチェックがすべて成功するまで待ちます。数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR のコメントやコード行へのインラインコメントで **@copilot** をタグ付けすると、Copilot に変更を依頼できます。生成結果は確率的であるため、依頼内容は状況に応じて決めます。例:

- 各コードファイルの先頭に、役割を説明するコメントヘッダーを追加します。
- TypeScript と Astro ファイルに TSDoc コメントを追加します。
- Astro アプリの構造を説明する README を作成します。

1. いずれかの依頼例、またはコードベースに必要な別のドキュメント変更を選び、`@copilot` をタグ付けしたコメントを追加します。
2. **View Session** を選択して、Copilot が変更する様子を確認します。更新用の新しいセッションが始まります。
3. **Back to pull request** を選択して PR に戻ります。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わったら、PR に新しいコミットがあることを確認します。
5. **Files changed** タブで変更をレビューします。

納得できるまで改善を続けます。完了したらドラフト PR をレビュー可能な状態にし、`main` にマージできます。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

1. GitHub.com のリポジトリで **Pull Requests** を選びます。
2. `Show related games on the game details page` などのタイトルが付いた PR を開きます。
3. **Files changed** タブを選んで生成コードをレビューします。
4. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
5. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

6. チェック欄でワークフローが実行され、単一 Astro アプリのプロジェクトチェックが成功することを確認します。
7. **省略可能:** Codespace でこのブランチに切り替えて関連ゲーム機能を手動テストできます。Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて、ターミナルで次を実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

関連ゲーム機能が実装されました。PR の **Conversation** タブにコメントを追加すると、コメントを見た Copilot が新しいセッションで変更を依頼できます。表示件数やコメントヘッダー、TSDoc コメントの追加を依頼する例があります。後者はカスタム指示を更新する前に割り当てたタスクであることに注意してください。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントが実装した機能をレビューします。この PR には Exercise 3 で依頼したハイコントラストモードと、Exercise 4 で追加依頼したライトモードが含まれます。

1. GitHub.com のリポジトリで **Pull Requests** を選びます。
2. `Add high contrast mode to website` などのタイトルが付いた PR を開きます。
3. Copilot がまだ作業中で **[WIP]** が付いている場合は、完了まで待ちます。
4. **Files changed** タブで変更をレビューします。
5. 次の実装を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保持する local storage
   - ハイコントラストモードとライトモードの CSS またはスタイル
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

8. チェック欄でワークフローが実行され、単一 Astro アプリのプロジェクトチェックが成功することを確認します。
9. **省略可能:** Codespace で Copilot が作成したブランチに切り替えて、アクセシビリティ機能を手動テストできます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

10. アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティエージェントが指針に沿って実装したことを確認します。改善点があれば、前の PR と同様にコメントで **@copilot** をタグ付けして依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent に実際のバックログを任せて別の作業に集中する方法を試すには、リポジトリに Issue を追加して Copilot に割り当てます。例:

- ゲーム詳細ページに支援者向けの関心フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

cloud agent のハーネスを完了しました。このレッスンでは、リポジトリのカスタム指示を確認し、Issue を割り当て、環境の準備と非同期作業を確認し、アクセシビリティ用カスタムエージェントを使い、エージェントページからセッションを監視して指示を出し、`@copilot` をタグ付けした PR コメントで変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

cloud agent のハーネスを完了しました。ほかの環境で同様の開発シナリオを試すには、次のハーネスに進みます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan、Autopilot、エージェントスキル、QA エージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、cloud agent にリファクタリングやテストカバレッジの Issue を割り当てたり、別分野のカスタムエージェントを作成したり、異なる技術スタック向けに `copilot-setup-steps.yml` を設定したりできます。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent ファイアウォールの設定][agent-firewall]

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
