---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー体験の改善と機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、進行中のタスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、タスクを管理するツールがあります。ここではオープン PR のあるエージェントタスクを確認し、実行中のセッションに追加の指示を出せます。

この演習では、エージェントページでタスクを監視し、実行中のセッションに機能追加を依頼します。

## シナリオ

ハイコントラストモードの実装中に、デザインチームはライトモードも必要だと気付きました。既存のスタイル更新と切り替え機能を活用できるよう、エージェントの作業にライトモードを追加します。

## Copilot cloud agent のタスクを確認する

まず、Copilot cloud agent に割り当てたタスクの状態を一覧で確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の一覧にあるタスクを確認します。ドキュメントの更新、ゲームを変更する API の生成、サイトへのハイコントラストモードの追加など、Copilot に依頼したタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。リポジトリのチェックアウト、環境の作成、MCP サーバーの設定、タスクを完了するための作業などが表示されます。

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が選んだ方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインで、PR と変更ファイルを確認します。

## cloud agent に指示を追加する

1. ハイコントラストモードを追加しているセッションを選択します。タイトルは Copilot が付けた名前や作業状態によって異なります。

   ![Mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページのタスクで、作業中のセッションに指示するダイアログを示す画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業に追加したことを確認します。

## Copilot の作業を続ける

Copilot は現在のステップを終えた後、新しい依頼を作業に取り込みます。完了までには数分かかる場合があります。その間に、これまでの内容を振り返ってください。

## まとめと次のステップ

このレッスンでは、Copilot agents ページでタスクを監視し、作業中のセッションに追加の機能を依頼しました。次は、[Copilot が作成した PR を反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
