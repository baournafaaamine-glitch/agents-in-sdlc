---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づくフィルターを実装し、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

小さな機能を追加した後は、より大きな変更に取り組みます。このレッスンでは、Issue を起点に新機能を計画し、コードを検証し、動作を確認して、プロジェクトにマージするまでのエージェント主導の開発プロセスを始めます。

> [!NOTE]
> 機能ワークフローでは、同じ会話とブランチを継続して使います。通常はファイル種別ごとにブランチやプルリクエスト (PR) を分けますが、このワークショップでは主要な概念に集中するため簡略化しています。

このレッスンでは、GitHub Issue から会話を開始し、Plan モードで要件を決め、Autopilot モードで実装し、コードをレビューして、転送ブラウザーで手動検証します。後続のレッスンでは、指示、スキル、MCP、QA エージェントを追加し、機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、利用者がカテゴリと発行元でゲームを絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、複数の条件を組み合わせる方法など、実装前に合意が必要です。Plan モードで決めてから、Autopilot に範囲を定めた実装を許可します。

## 背景

AI コーディングエージェントを導入しても、開発の基本は変わりません。多くの開発者は次のような流れで作業します。

1. 実装内容を記述した Issue から始めます。
2. 実装計画を立てます。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新しい機能を手動で確認します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーション (CI) が成功した後にマージします。

チームや組織によって詳細は異なりますが、これに近い手順が一般的です。この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手書きのコードと同じレビューを通過するようにします。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> で切り替えます。

- **Interactive**: エージェントが変更を提案し、次に進む前に開発者の入力を待ちます。
- **Plan**: エージェントが計画を作成しますが、プロジェクトファイルは編集できません。
- **Autopilot**: エージェントがコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルで `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

4. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

5. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
6. <kbd>A</kbd> を押してすべての Issue を表示し、矢印キーで **Allow users to filter games by category and publisher** を選びます。
7. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。プロンプトに `#7` などの Issue 番号が表示されます。`#` を使うと GitHub Issue や PR をコンテキストに含められます。

## フィルター機能を計画する

複雑なタスクでは、実装前に方針を決めます。Plan モードに切り替え、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードにし、プロンプトの下に **Plan** と表示されることを確認します。
2. Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

3. フィルターの動作について質問されたら、希望する仕様を回答します。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を読み、必要に応じて修正を依頼します。

## Autopilot を承認する

計画を確認したら、Autopilot に実装を開始させます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様のラベルの選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐ始まる場合があります。先に計画を確認してください。依存関係不足やポート競合が報告された場合は、セットアップの問題を解決してからチェック結果を完了と判断します。

## 実装をレビューして検証する

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> で Interactive モードに切り替え、モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. Issue と計画中に決めた仕様に照らして変更を確認します。
4. <kbd>Esc</kbd> で差分ビューを閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば修正を依頼します。

## 新機能を試す

1. 次の依頼でアプリを起動します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. 計画で合意したカテゴリ、発行元、組み合わせのフィルターを試します。
4. リセットと検索結果なしの場合の動作が Issue と合意内容に一致することを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

このレッスンでは、会話モードを使い分けて機能を構築し、レビューしました。次は[カスタム指示を使って][next-lesson]コードが文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
