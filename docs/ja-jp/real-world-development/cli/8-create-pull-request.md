---
title: "レッスン 8 - 機能プルリクエスト (PR) の作成とマージ"
description: "フィルター、指示、スキル、QA プロファイル、テストを確認して PR を作成し、Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、指示の更新、スキルの変更、品質保証 (QA) プロファイル、テストはすべて同じブランチにあります。まとめてレビューし、プルリクエストを作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は機能、指示、スキル、QA エージェントを複数の PR に分けます。このワークショップでは作業を簡略化するため、フィルターと品質ワークフローを 1 つの会話とブランチにまとめています。

このレッスンでは、Agent Merge の仕組みを確認し、変更と検証の証拠を調べ、フィルター PR を作成して、レビュー後に Agent Merge を有効にします。

## シナリオ

フィルター機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージを許可するかどうかは開発者が判断しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、PR をマージするまでに必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗修正、レビューコメントへの対応、必要なリベースなど、PR をブロックしている問題に取り組みます。その後、リポジトリが許可していれば GitHub の自動マージを有効にします。

有効化する前に作業をレビューし、準備ができていると判断してください。

## Agent Merge で PR を管理する

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` を実行して PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認し、すべて成功した後にマージすることを確認します。
7. `PR #14 was squash-merged successfully.` など、マージ成功を示すメッセージを確認します。

> [!IMPORTANT]
> Agent Merge は必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は理由を確認し、リポジトリで許可されていればレビュー済みのマージを手動で行います。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストのプロセスを自動化しました。次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使う[便利な Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
