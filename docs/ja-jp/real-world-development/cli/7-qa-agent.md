---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。今度は、明確な品質保証 (QA) プロセスを持つカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、カスタムエージェントと指示、スキル、MCP ツールの連携を確認し、再利用可能な QA プロファイルを作成して、フィルター Issue に照らした結果をレビューします。

## シナリオ

Tailspin Toys では PR を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントが QA プロセスを調整し、再利用できるレポートを作成します。

## カスタムエージェントとは

カスタムエージェントは、目的、指示、利用可能なツールを Markdown プロファイルで定義する Copilot の専門役です。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

各カスタマイズの役割は異なります。リポジトリの指示はチームの標準を示し、`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供し、QA プロファイルはそれらを使って要件を評価し、結果を報告する方法を指示します。

## QA プロファイルを作成する

機能 PR を作成する前に、実行するチェックと守るべき境界を定義したプロファイルを Copilot に作成してもらいます。

1. Codespace に戻り、フィルター会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のプロンプトを送信します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

エージェントを使う前に、意図した QA の流れと制約が記述されているか確認します。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は任意ですが、含めると表示名が明確になります。
3. 要件から確認を始め、リポジトリ指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うよう記述されていることを確認します。
4. 根拠を示して報告し、実装コードの変更前に確認を求め、コミットや PR 作成を行わないことを確認します。
5. 責務や制約が不足している場合は、続ける前に既定のエージェントへ修正を依頼します。

## Issue を QA する

Copilot CLI は会話の開始時にプロジェクトエージェントを読み込みます。プロファイル作成後に同じフィルター会話を再開し、QA を選ぶと、Issue と計画のコンテキストを引き継げます。

1. 次のコマンドで QA エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 新しく作成したエージェントが選択肢に表示されない場合があります。その場合も、上記コマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を確認します。
4. 完了後、レポートを読みます。

## まとめと次のステップ

再利用可能な専門役をワークフローに加え、その結果を確認しました。次は[機能 PR に変更をまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタムエージェントの作成][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
