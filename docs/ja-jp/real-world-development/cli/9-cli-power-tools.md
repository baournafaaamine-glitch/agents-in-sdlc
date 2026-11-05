---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト、モデル選択、セッション共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、操作、詳細情報、設定に使う多くのスラッシュコマンドがあります。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、`/context` と `/compact` で会話コンテキストを調べ、`/model` で利用可能なモデルを確認し、`/share` でセッションを共有します。また、並列作業、worktree、cloud agent への委任に使う任意のコマンドを紹介します。

## シナリオ

CLI の主要なワークフローを終えたので、コンテキスト管理、モデル切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への委任を見ていきます。

## Copilot CLI のコンテキストを確認する

大きなタスクでは、モデルのコンテキストウィンドウが上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って状態を確認したり、手動で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していなければリポジトリのルートから起動します。
2. `/context` を入力します。
3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き容量の内訳を確認します。
4. `/compact` を入力して会話を圧縮します。
5. もう一度 `/context` を入力して比較します。会話が短い場合、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると Copilot CLI は自動的に圧縮します。タイミングを指定するには `/compact` を使います。無関係なタスクに移るときは `/clear` または `/new` で新しい会話を始めます。

## モデルを選ぶ

モデルごとに得意分野が異なります。Copilot CLI でモデル一覧を確認し、使用するモデルを選びます。

1. `/model` を入力します。
2. 使用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

セッションを共有する方法は複数あります。`/share` コマンドで Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. 共有する場合は、たとえばローカルの Markdown に出力する `/share file` など、用途に合う方法を選びます。
3. 送信または公開する前に、エクスポート内容を確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並列化または委任する

主要なワークショップは完了です。Copilot CLI には大きなタスク向けのコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに割り当て、並列実行できます。
- `/worktree` は別タスク用の独立した Git worktree を作成できます。
- `/delegate` は非同期に作業する Copilot cloud agent にタスクを送信し、PR を作成させることができます。

これらのコマンドは追加の worktree やリモート作業を作る可能性があるため、任意です。範囲の明確な新しいタスクから始め、通常の手順で結果をレビューしてください。非同期エージェントの作業を詳しく確認する場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

スラッシュコマンドで Copilot CLI の設定、セッション共有、内部状態の確認を行いました。次は[ワークフローを振り返り][next-lesson]、今後の学習方法を確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
