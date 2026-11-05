---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。プルリクエスト (PR) を開く前に、ブラウザーで正しく動作するか確認します。**Playwright MCP サーバー**を接続し、Copilot にブラウザーを操作させて機能をテストし、最後にブランチを公開して PR を作成します。

この演習では、Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を確認し、Playwright MCP をワークスペースに加えてブラウザーテストに使い、フィルター変更の PR を作成します。

## Model Context Protocol (MCP) とは

エージェントモードはエディターの外部に接続するとさらに役立ちます。Model Context Protocol (MCP) は、エージェントが外部ツールやサービスと通信するための標準です。

![エージェントモード、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールの関係を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部ツールやサービスとリアルタイムに通信し、最新情報へのアクセスや操作を可能にします。MCP サーバーはエージェントと外部ツールの橋渡しをし、利用可能なツールとリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)** は、GitHub リポジトリ、Issue、プルリクエストなどを管理する API を利用できるようにします。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)** は、Playwright によるブラウザー自動化を提供します。

GitHub は MCP サーバーの発見や貢献に使える [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> MCP サーバーは他の依存関係と同様に扱ってください。使用前にソースコードと発行元を確認し、セキュリティへの影響を検討します。信頼できるサーバーだけを使い、機密リソースや操作へのアクセス許可に注意してください。

## MCP 設定を確認する

`.vscode/mcp.json` は、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに設定された `github` サーバーを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクトテンプレートに含まれています。前の演習で Copilot が Issue のバックログを読めたのはこのためです。[リモート GitHub MCP サーバー][remote-github-mcp-server]を HTTP で使い、ツールの初回利用時に GitHub で認証します。ローカルへのインストールは不要です。

## Playwright MCP サーバーを追加する

[Playwright MCP サーバー][playwright-mcp-server]を追加すると、Copilot が操作できるブラウザーを利用できます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

`github` はリモートサーバーですが、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面を表示せずにブラウザーを動かす設定で、デスクトップ環境のない Codespace で必要です。

> [!NOTE]
> Tailspin Toys は E2E テストで Playwright を使うため、ブラウザーは通常インストール済みです。ブラウザーがないと Copilot が報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。初めてサーバーのツールが必要になったとき、VS Code が起動して信頼を確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** を選択し、設定が正しいことを確認します。
2. 信頼の確認を求められたら、設定を確認し、サーバーを起動するため信頼する選択肢を選びます。

手動で起動する手順は省略できます。その場合、次の操作でツールが必要になったときに VS Code が起動し、信頼を確認します。

## フィルター機能をテストする

Playwright MCP サーバーを使うと、エージェントがページを開き、移動し、フィルターを適用して、結果を確認できます。サーバーを接続し、アプリが起動している状態で、Copilot に次を依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Playwright MCP サーバー経由で Copilot がブラウザーを起動し、各手順を確認して結果を報告します。レポートを Issue の受け入れ条件と照らし、問題があれば PR 作成前に追加の質問やコード修正を依頼します。

> [!NOTE]
> テスト時はアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP の初回利用時にブラウザーが必要だと報告された場合は `npx playwright install chromium` を実行して再試行します。

## ブランチを公開して PR を作成する

機能の動作を確認したら、チームのレビュー用にブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot による変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![Copilot が行った変更を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

Copilot に GitHub ツールを使って関連 Issue を探し、現在のブランチから PR を作成してもらいます。

1. Copilot Chat パネルで **New Chat** を選び、新しいセッションを開始します。
2. エージェント選択メニューで **Agent** を選択します。
3. 次の依頼を送信します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を許可します。GitHub ツールの初回利用時に認証を求められたら、案内に従います。
5. Copilot が Issue を検索し、正しい Issue に関連付けて PR を作成することを確認します。
6. Copilot が作成したリンクを選んで PR をレビューします。まだマージしないでください。

## まとめと次のステップ

この演習では、MCP と MCP サーバーが Copilot にツールを追加する仕組みを確認し、Playwright MCP を使ってブラウザーでフィルター機能をテストし、ブランチを公開して PR を作成しました。次は[カスタムエージェントを作成][next-lesson]し、アクセシビリティレビューを効率化します。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
