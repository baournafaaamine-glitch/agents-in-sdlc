---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルター機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く以外にも、データや外部サービスとの連携、Copilot が利用する自動化ツールが必要です。MCP サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスを追加できます。

このレッスンでは、Model Context Protocol (MCP) と Copilot CLI での使い方を確認し、必要なら Playwright MCP サーバーを追加して、エージェントにブラウザーから機能を試してもらいます。

## シナリオ

単体テストとエンドツーエンドテストは重要ですが、UI の変更を検証するには実際に操作する必要があります。利用者と同じようにサイトを操作するため、Copilot に Web サイトを使わせ、変更が期待どおりに動くかを確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部ツールやサービスと通信するための仕組みです。MCP を通じて最新情報にアクセスしたり、依頼に応じて操作を実行したりできます。

MCP サーバーはエージェントと外部ツールの橋渡しをし、エージェントが利用できるツールやリソースを提供します。たとえば、[GitHub MCP Server][github-mcp] はリポジトリ、Issue、プルリクエストを管理する API を利用できるようにし、[Playwright MCP Server][playwright-mcp-server] は Playwright によるブラウザー自動化を提供します。GitHub はエコシステムの発見と貢献を促す [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは他の依存関係と同様に扱ってください。使用前にソースコードと発行元を確認し、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP がまだ Copilot CLI で利用できない場合は、サイトを操作できるように追加します。

> [!NOTE]
> Playwright MCP がすでに設定されている場合は、セットアップを飛ばして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバー種別が **STDIO** であることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを入力します。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) で保存します。
7. <kbd>Esc</kbd> で MCP ダイアログを閉じます。

## Playwright で機能を確認する

前のレッスンでは機能を手動で確認しました。今度は Copilot が利用できる Playwright MCP サーバーで同じ確認を行います。

1. 次のプロンプトで、MCP サーバーを使った検証を依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

2. Copilot がブラウザーで機能を確認する手順を見ます。
3. レポートを読み、すべて期待どおりに動くか確認します。

Copilot はサーバーを起動して Playwright でサイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

このレッスンでは、Playwright MCP を使って Copilot CLI から実際のブラウザーで機能を確認しました。次は[QA カスタムエージェントを作成][next-lesson]し、スキルとブラウザーツールを専門的な役割にまとめます。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
