---
title: "レッスン 5 - quality-checks スキルを調整して使う"
description: "既存の quality-checks スキルを確認し、レポート形式を調整して、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、テスト、lint、継続的インテグレーション (CI) などの確認が必要です。**Agent skill** は、こうしたタスクを実行する方法を Copilot に伝えるのに適しています。

このレッスンでは、既存の `quality-checks` スキルを確認し、結果の形式を調整してから再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためレポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめ、必要なときにエージェントが読み込むものです。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルにはスキルの名前と説明を定義する frontmatter、概要、呼び出すタイミングの指針があります。必要に応じて、スクリプトやリソースのサブフォルダーも含められます。

> [!NOTE]
> スキルに追加のファイルやフォルダーは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため `SKILL.md` のみで構成されています。

プロジェクトの `.github/skills` に置くとチームで共有でき、ユーザーの `~/.copilot/skills` に置くと個人用になります。

## スキルを確認する

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明によって Copilot がスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の手順を確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

1. Interactive モードでフィルター会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポート形式を調整する

スキルのレポートに、実行内容、成否、ツールが報告した内容を含めるようにします。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

ファイルは自動的に保存されます。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 新しいレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

このレッスンでは、`quality-checks` スキルを確認し、結果の形式を調整して、再読み込み後に実行しました。変更は機能 PR に含めます。次は[Playwright MCP サーバーを使って][next-lesson]Copilot がサイトを直接操作できるようにします。

## スキルの例

以下は参考例であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
