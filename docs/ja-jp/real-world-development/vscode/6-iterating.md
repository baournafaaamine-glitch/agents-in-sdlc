---
title: "演習 6 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
| :-- |

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加に取り組みました。エージェントモードでクライアントとサーバーの両方にフィルターを追加し、Playwright MCP サーバーでブラウザーから手動テストしました。その後、アクセシビリティ用カスタムエージェントでハイコントラストとライトモードの切り替えを実装し、進行中のセッションに追加の指示を出しました。次はローカルの変更を公開し、チームと同じ手順でレビューします。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。Copilot が生成したコードは、通常のレビューを経る必要があります。Codespace からアクセシビリティの変更をプッシュし、PR を作成して、チームのレビューに向けて差分を確認します。

## アクセシビリティ機能を公開する

[演習 4][exercise-4] と[演習 5][exercise-5]で作成したハイコントラストとライトモードの切り替えは、Codespace にコミット済みの変更としてあります。ブランチをプッシュして PR を作成します。

1. Codespace に戻ります。
2. VS Code で **Source Control** ビューを開きます。
3. アクセシビリティの変更がコミット済みであることを確認します。演習 5 の未コミットの変更がある場合はステージし、`Add high-contrast and light-mode toggles` などの内容を示すメッセージでコミットします。
4. **Publish Branch** を選択してブランチを公開します。または **...** メニューから **Push** を選びます。
5. VS Code の案内に従って github.com で新しいブランチを開くか、リポジトリに移動してブランチのバナーにある **Compare & pull request** を選択します。
6. `Add high-contrast and light-mode toggles` などの明確なタイトルと、変更内容と理由をまとめた説明を入力します。
7. **Create pull request** を選択します。
8. PR の **Files changed** タブを選び、作業全体を確認します。次の点を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保持する local storage
   - ハイコントラストモードとライトモードの CSS またはスタイル
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript
9. **Conversation** タブに戻ります。
10. 承認待ちのワークフローがある場合は、**Approve and run workflows** を選択します。

    ![ワークフローの承認と実行を行う Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. ワークフローが完了するまで待ち、成功したことを確認します。

> [!TIP]
> アクセシビリティの変更について別の意見が必要な場合は、PR コメントで `@copilot` をタグ付けし、「ほかに WCAG の問題がないか確認してください」や「キーボード操作の改善を提案してください」などと依頼します。Copilot はコメントに対応する新しいセッションを開始します。

## 任意の演習 — ローカルでさらに試す

IDE 内でエージェントと反復して作業するには、練習を重ねることが大切です。VS Code で次のような追加作業を試せます。

- ゲーム詳細ページに支援者向けの関心フォームを追加します。
- ゲーム一覧ページにページネーションを実装します。
- `src/lib/` のデータアクセスヘルパーに入力検証とエラー処理を追加します。
- アクセシビリティエージェントの対象を広げ、サイト全体のキーボードフォーカス順序を監査します。

## まとめ

VS Code ハーネスを完了しました。このラボでは、次のことを行いました。

- **Playwright MCP で機能を手動テストしました。** Playwright MCP サーバーを追加し、PR の作成前に Copilot にブラウザーを操作させてフィルター機能を確認しました。
- **エージェントモードでスタック全体の変更を調整しました。** 1 つのセッションでクライアント、サーバー、テストにまたがるフィルター機能を追加しました。
- **カスタムエージェントを使いました。** エージェント選択メニューからアクセシビリティエージェントを選び、高コントラストモードの実装に使いました。
- **エージェントセッションを管理して方向付けました。** 変更をインラインでレビューし、必要なものを受け入れ、ライトモードの依頼でセッションを広げました。
- **PR で作業をまとめました。** ローカルの変更を公開して、チームと同じ手順でレビューしました。

## 振り返りと次のステップ

必須の VS Code ハーネスは完了です。ここでワークショップを終えてもかまいません。

Copilot のエージェント機能を別の環境でも試す場合は、次のハーネスを確認してください。

- 🖥️ **[CLI ハーネス](../../cli/)** — Copilot CLI でターミナルから同様の作業に取り組みます。Plan モード、エージェントスキル、カスタムエージェント、`/share`、`/context`、`/delegate` などのスラッシュコマンドを扱います。
- 💻 **[クラウドエージェントハーネス](../../cloud/)** — Issue の割り当て、エージェントページでのセッション監視、PR での非同期の反復作業を扱います。

[awesome-copilot][awesome-copilot] の指示ファイル、カスタムエージェント、スキルを、自分のプロジェクトに応用することもできます。

任意の拡張演習である[Foundry を組み込む][exercise-7]では、VS Code と Foundry Toolkit を使ってモデルを準備し、Backer Concierge をデプロイしてサイトに接続します。

## リソース

- [GitHub Copilot][github-copilot]
- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェントモードの使用][agent-mode]

---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
