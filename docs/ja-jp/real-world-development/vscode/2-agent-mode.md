---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

簡単な更新でも、複数のファイル変更やテスト実行が必要になる場合があります。Copilot エージェントモードは統合開発環境 (IDE) 内でプロジェクトを調べ、関連ファイルを特定し、必要な変更やテストを行い、見つかった問題を修正します。この演習では、エージェントモードでサイトに機能を追加します。

> [!NOTE]
> エージェントモードと cloud agent は異なる体験です。エージェントモードは IDE 内で作業し、すばやいフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエスト (PR) を通じてやり取りします。

この演習では、プロジェクトを調べて関連ファイルを特定する方法、ユーザーインターフェイス (UI) とデータ層にまたがる機能の実装方法、変更とテストをレビューする方法を確認します。

## シナリオ

ゲーム一覧をカテゴリと発行元で絞り込めるようにします。前の演習で追加した publishers ヘルパーを使い、残りのデータ層、UI、テストを Copilot エージェントモードで仕上げます。

## Tailspin Toys の Web サイトを起動する

変更前に、サイトの現在の機能を確認します。Tailspin Toys は開発者をテーマにしたボードゲームのクラウドファンディングサイトです。単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で実装します。

### Web サイトを起動する

Codespace の `npm run dev` タスクで、単一の Astro 開発サーバーを起動します。

1. Codespace に戻ります。前の演習から `main` にいることを確認します。
2. Codespace で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押してターミナルを開きます。
3. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

### Web サイトを確認する

サイトが起動したら、現在の機能を確認します。

1. ゲーム一覧を開き、タイトル、画像、説明を含むボードゲームの一覧を確認します。
2. ゲームを選択し、タイトル、説明、発行元、カテゴリを含む詳細ページを確認します。

Codespace のターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストする Codespace へのプライベートトンネルです。

## Copilot でバックログを確認する

フィルター機能を実装する前に、Copilot に Issue のバックログを調べてもらいます。

1. Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択します。
2. Chat ビューのエージェント選択メニューで **Agent** を選びます。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

3. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> このワークショップは一貫した体験を提供するために Claude Sonnet 4.5 を使って作成されています。特定のモデルを推奨するものではありません。大規模言語モデル (LLM) は確率的に動作するため、提案は例と異なる場合があります。これは通常の動作です。

4. 次のプロンプトを送信し、バックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

5. Issue の一覧を取得するため **Continue** を選択します。
6. Copilot が生成した Issue 一覧と優先順位を確認します。

## 指示ファイルを確認する

コード生成を始める前に、Copilot にコンテキストを与える UI 指示ファイルを確認します。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. アーキテクチャの概要、コンポーネント設計、テスト可能性、アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などの指示へのリンクを確認します。

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクトファイルを参照できます。パスは指示ファイルからの相対パスです。これにより、複雑な指示を再利用できる小さな単位に分け、例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能には、データ層のロジック、動作を確認するテスト、ゲーム一覧の UI が必要です。Copilot エージェントモードに実装を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択します。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。
4. **Claude Sonnet 4.5** が選択されていることを確認します。
5. 次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データ層、UI、テストを特定して変更を加える様子を確認します。
7. テスト実行を求められたら **Continue** を選択します。途中でテストが失敗しても、Copilot はコード生成とテストを反復します。提案された変更は完了後に確認します。
8. 生成コードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

9. ブラウザーでサイトを開き、新しいフィルター機能を試します。
10. 動作とコードを確認した後、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、Copilot エージェントモードで UI とデータ層に機能を追加し、プロジェクト内の関連ファイルを特定して変更し、生成コードとテストをレビューしました。次は[Playwright MCP サーバーで機能を確認し][next-lesson]、PR を作成します。

### 追加演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になる場合があります。この演習で学んだ方法を使い、Copilot にページネーションを依頼できます。既存の指示を使い、ページサイズを利用者が選べるようにするか固定するかを決め、必要なコンテキストをプロンプトに含めます。結果が希望どおりになるまで Copilot に変更を依頼してください。

## リソース

- [Copilot の Ask、Edit、Agent モードの違いと使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
