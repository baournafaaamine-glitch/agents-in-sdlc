---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムエージェント][previous-lesson] |
|:--|

エージェントモードの Copilot は、コードベースを調べ、変更を提案し、ファイルを編集してコマンドを実行します。VS Code の Copilot Chat では、ツール呼び出しやファイル編集をリアルタイムで確認できます。差分をインラインでレビューし、個々の変更を受け入れるか戻すかを選び、エディターを離れずに追加プロンプトを送れます。

この演習では、アクセシビリティエージェントの作業を監視し、変更をレビューして受け入れ、進行中のセッションにライトモードのトグルを追加するよう依頼します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティカスタムエージェントがハイコントラストモードを実装しています。作業中、デザインチームから既定のテーマが暗すぎる利用者向けにライトモードも必要だと依頼がありました。新しい会話を始めず、既存セッションの作業範囲を広げます。

## Copilot の進行状況を監視する

Copilot Chat ビューには、エージェントが調べたファイル、実行したコマンド、提案した編集が表示されます。編集は差分と **Keep** / **Undo** の操作とともにインラインで表示されます。

1. 演習 4 で使った Copilot Chat ビューに戻ります。
2. エージェントの作業中にチャットをスクロールし、ツール呼び出し、ファイル編集、実行許可を求めるターミナルコマンドを確認します。
3. ファイル編集が提案されたら、**Keep** で変更を受け入れるか、**Undo** で戻して再試行させます。
4. チャット上部の **Files changed** を開き、このセッションで変更されたファイルを一覧します。

> [!NOTE]
> ツール呼び出しの許可 (たとえばターミナルコマンドの実行) を求められると、エージェントは **Allow** を表示して一時停止します。内容を確認し、許可するコマンドを選んでください。

5. エージェントがハイコントラストモードの実装を完了し、変更の要約を表示するまで待ちます。

## ライトモードを追加するようエージェントに指示する

同じ会話を続けると、エージェントは変更済みコンポーネント、スタイル、local storage の使い方などのコンテキストを保持します。

1. Copilot Chat の入力欄に次の追加プロンプトを送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. エージェントが既存の変更を調べ、ライトモードの変更を提案する様子を確認します。
3. 各差分をレビューし、**Keep** または **Undo** を選びます。
4. 望まない方向に進んだ場合は、チャット入力欄の隣にある **Stop** で停止し、補足の依頼を送ります。

> [!TIP]
> 進行中のセッションに追加の指示を送ると、最初からやり直すより効率的です。たとえば、CSS を簡潔にする、既存の設定パネルに統合する、両方のトグルにキーボードショートカットを追加するなどの調整を依頼できます。

## 変更されたファイルをレビューする

コミット前にエージェントが変更したファイルを確認します。

1. VS Code で **Source Control** ビューを開きます。
2. 変更されたファイルの一覧を確認します。Astro コンポーネント、スタイル、関連テストなどが含まれます。
3. 変更されたファイルを開いて差分を確認します。ARIA 属性、キーボード操作、セマンティック HTML、local storage による設定保持などを確認します。
4. 内容に問題がなければ **Source Control** パネルで変更をステージしてコミットします。次のレッスンで公開します。

## まとめと次のステップ

このレッスンでは、Copilot Chat のエージェントモードで進行中のセッションを監視し、個々のファイル編集をレビューして受け入れるか戻すかを選び、追加プロンプトで作業範囲を広げました。次は[アクセシビリティの変更を PR として公開][next-lesson]し、このラボで作成したローカルカスタムエージェントの変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェントモードの使用][agent-mode]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
