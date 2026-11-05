---
title: "レッスン 2 - 星評価を追加する: 小さな成果"
description: "Copilot CLI でゲームカードを変更し、転送されたブラウザーで確認して、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、最初の変更を加えます。ゲームデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更を確認して、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を開始します。
- 小さな変更を依頼し、`/diff` でレビューします。
- 転送されたブラウザーでアプリを確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

各ゲームの星評価は、ゲーム詳細ページには表示されていますが、ホームページのカードにはタイトル、カテゴリ、発行元、説明しか表示されていません。既存の評価をカードにも表示する、小さく独立した変更を行います。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。通常の CLI 会話では、Copilot app のように専用 worktree を作成するのではなく、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話には後から戻れますが、ファイルとブランチはディスク上の通常の Git 状態です。

会話には、プロンプトとエージェントの応答、ファイルの調査や編集などのツール操作、`/diff` で確認できる変更が表示されます。

## 会話を開始して変更を依頼する

Copilot CLI の会話は、現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話には後から戻れますが、ファイルとブランチは通常の Git 状態のままです。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. Copilot CLI が起動していなければ、次のコマンドで起動します。

   ```bash
   copilot --yolo
   ```

4. `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

## 差分をレビューする

小さな変更であっても、マージ前に AI が生成した変更をレビューします。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値を、`starRating` が `null` の場合は `No rating yet` をカードに表示することを確認します。
3. テストが両方の状態を確認していることを確かめます。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビュー後、<kbd>Esc</kbd> を押して差分画面を閉じます。

> [!NOTE]
> Copilot を含む生成 AI の出力は確率的であり、コードは例と異なる場合があります。特定の実装ではなく、動作を確認してください。

## 変更を確認する

コードを読むだけでなく、Codespaces が転送するブラウザーで更新後の UI を確認します。

1. 次のプロンプトでアプリを起動します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. ゲームカードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、次のプロンプトで起動したサーバーを停止します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

変更を確認できたら、プルリクエストを作成してマージし、ローカルの `main` を更新します。

1. 既定のエージェントに、レビュー済み変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI はプルリクエストの作成時に既存のコミットをプッシュし、URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら URL を選択します。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確認します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

Copilot CLI で最初の変更をリリースしました。このレッスンでは、機能ブランチで会話を開始し、変更を依頼し、`/diff` で確認し、転送ブラウザーで評価を検証して、プルリクエストを作成・マージしました。

次は、[フィルター Issue を起点に Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
