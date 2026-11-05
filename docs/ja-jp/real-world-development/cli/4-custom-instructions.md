---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント作成標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、必要なコンテキストを Copilot が参照できるようにすることが重要です。[指示ファイル][instruction-files]では、作りたいコードだけでなく、その構成方法も伝えられます。フィルター機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、リポジトリ指示とパス指定の指示ファイルがエージェントに適用される仕組みを確認し、指示ファイルを更新してコードへの影響を確かめます。

## シナリオ

Tailspin Toys では、コメントで意図や自明でない判断を説明し、コードを言い換えるだけのコメントは避けます。`db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値 (注入可能な `db` 引数がある場合はそれも含む) を TSDoc/JSDoc で記述します。再利用可能な Astro コンポーネントの `Props` 契約も文書化し、関連コードの変更時にはコメントを最新に保ちます。既存の書式と lint の指針も維持します。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件を Copilot に伝えます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストに送られるプロジェクト全体向けの指示です。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクごとの指針を記述します。

> [!NOTE]
> 指示形式とサポート状況は環境によって異なります。特定の形式を前提にする前に、[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## カスタム指示ファイルを管理するためのベストプラクティス

このラボのサンプルは、指示ファイルを管理する方法の一例です。プロジェクトの概要、構成、全体的なコーディング標準などの情報は `copilot-instructions.md` にまとめます。単体テスト、Astro コンポーネント、データ層など、特定のファイル種別やタスクの指示には `*.instructions.md` を使います。自然な言葉で明確に書き、コードの良い例と避ける例を示します。

指示ファイルを作成する唯一の方法はありません。プロジェクトで試しながら、適した方法を見つけてください。

## プロジェクトの指示ファイルを確認する

スタータープロジェクトに含まれる指示ファイルを調べ、変更前の状態を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング指針を確認します。これらはこのリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` を開き、Astro、Drizzle データ層、テストなどの指示ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` が適用対象のファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` のリンクから、再利用可能な指示ファイルやプロジェクト内の例を確認します。

## チームの方針に合わせて指示を更新する

新しいドキュメント標準を Copilot に適用させるため、プロジェクト全体向けの指示に規則を追加します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション (35 行目付近) を探します。
2. 最後の箇条書きとして、次の指示を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を適用するため、会話を終了してフィルター会話を再開します。

1. フィルター会話が開いている場合は `/exit` で終了します。会話は自動保存されます。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替わっていることを確認し、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたらフィルター会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 次のプロンプトで、更新した指針の適用を依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。TSDoc コメントが追加され、コードを正確に説明していることを確かめます。

## まとめと次のステップ

このレッスンでは、Copilot が指示ファイルのコンテキストを取り込む方法を確認し、新しい標準を機能に適用しました。次は、[再利用可能な quality-checks スキルを調整して実行][next-lesson]し、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
