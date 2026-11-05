---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシーコード、バックログに残った機能要望など、技術的負債に直面しています。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に行い、作業後に人間がレビューできるドラフトプルリクエスト (PR) を作成します。開発者は時間のかかる作業を任せ、より大きな課題に集中できます。

この演習では、コード生成環境のカスタマイズ、安全な操作、範囲を明確に定めた Issue の重要性、Issue の Copilot への割り当てを扱います。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。まず、エクスポートされた関数に TSDoc コメントを追加します。また、ゲーム詳細ページに同じカテゴリの関連ゲームを表示し、デザインチームがユーザー体験 (UX) の受け入れテストに使えるようにします。ほかの優先度の高い作業があるため、これらの作業を Copilot cloud agent に非同期で任せます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) はバックグラウンドで作業します。[GitHub Issue を Copilot に割り当てる][assign-issue]と、Copilot は進行状況を追跡するドラフト PR を作成し、環境を準備してタスクに取り組みます。作業中または完了後にセッションを確認でき、レビューが必要になると PR で通知されます。

## 範囲を明確にした指示の重要性

Copilot を使っても、開発者の判断が不要になるわけではありません。タスクの範囲を明確にし、段階的に進め、結果を学びながら調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。[cloud agent に依頼するタスクの範囲を慎重に定めてください][cloud-agent-best-practices]。

## このリポジトリのカスタム指示

前の演習でカスタム指示を確認しました。cloud agent に作業を割り当てる前に、リポジトリの指示を読み、後で影響を確認できるようにします。

1. リポジトリの `.github/copilot-instructions.md` を開き、特に **Code standards** の TypeScript 規約、TSDoc コメントを確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` frontmatter が `**/*.test.ts` に指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を割り当てたら、生成された PR に TSDoc コメント、TypeScript 規約、コメントヘッダーが反映されているか確認します。

## Copilot cloud agent 用の開発環境を設定する

cloud agent は GitHub Actions の環境で作業します。`.github/workflows/copilot-setup-steps.yml` に専用のセットアップワークフローを置くと、必要な開発ツールや依存関係を準備できます。このラボでは、Node.js、プロジェクト依存関係、エンドツーエンド (E2E) テスト用ブラウザー、移行とシードが済んだ SQLite データベースを使えるように事前設定されています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot は自身の操作に専用トークンを使用します。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` 権限が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

このワークフローには `copilot-setup-steps` という 1 つのジョブがあり、Copilot が PR 作業を始める前に GitHub Actions で実行されます。`workflow_dispatch` により、リポジトリの **Actions** タブから手動実行し、Copilot を待たずに設定をテストできます。

## ドキュメントを追加する

ドキュメントの不足は、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot cloud agent にコードドキュメントの Issue を割り当て、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブ、**New issue**、**Blank issue** の順に選択します。
3. **Title** に `Code lacks documentation` と入力します。
4. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

5. **Create** を選択して Issue を作成します。
6. 右側の **Assign to Copilot** を選択し、ダイアログで **Assign** を選択します。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

7. **Pull Requests** タブを選択し、作成された PR を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。表示されない場合は少し待ってから再読み込みします。
8. 数分後、Copilot が作成した TODO リストを確認します。

> [!NOTE]
> TODO リストが表示されるまで数分かかる場合があります。Copilot が環境を準備し、プロジェクトを分析して、作業方針を決めています。

9. TODO リストを読み、実行するタスクを確認します。
10. PR のタイムラインを確認し、Copilot が Issue に取り組み始めたことを確認します。
11. **View session** を選択してセッションを表示します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

12. セッションを読み、Copilot がコードを調査して方針を決め、コードを作成する様子を確認します。最新の状態が表示されない場合は、ブラウザーを再読み込みします。

作業には数分かかる場合があります。cloud agent が非同期で作業している間に、別のタスクを進められます。

## ゲーム詳細ページに関連ゲームを追加する

別の作業を並行して進めるため、関連ゲーム機能も cloud agent に割り当てます。

1. github.com のリポジトリに戻ります。
2. **Issues**、**New issue**、**Blank issue** の順に選択します。
3. **Title** に `Show related games on the game details page` と入力します。
4. **Description** に次の要件を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. **Create** を選択して Issue を作成します。
6. 右側の **Assign to Copilot**、続いて **Assign** を選択します。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

Issue の最初のコメントに 👀 が表示されると、Copilot が作業を開始したことが分かります。完了後に結果をレビューします。次はカスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent に Issue を非同期で割り当て、技術的負債の解消や機能追加を任せる方法を確認しました。次は、[カスタムエージェントを作成して使います][next-lesson]。cloud agent は [MCP サーバー][cloud-agent-mcp]とカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
