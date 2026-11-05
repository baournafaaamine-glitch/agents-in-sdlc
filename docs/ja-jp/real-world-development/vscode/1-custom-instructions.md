---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、Copilot が適切なコンテキストを参照できるようにすることが重要です。[指示ファイル][instruction-files]を使うと、コード生成の進め方を伝えられます。

この演習では、リポジトリ指示とパス指定の指示ファイルを確認し、現在の指示でフィルター用データスライス (publishers ヘルパー) を作成します。次に `.github/copilot-instructions.md` に標準を追加して同じプロンプトを再実行し、変更を `main` に反映します。

> [!CAUTION]
> Copilot の生成結果は確率的なため、設定した標準から外れる場合があります。ここでは、指示の更新後に傾向がどう変わるかを確認します。出力が例と一字一句一致することは目的ではありません。

## 指示ファイル

### シナリオ

Tailspin Toys の開発ガイドラインには、データ層に単体テストを設けること、UI をモダンなダークモードにすること、TSDoc コメントでコードを文書化すること、各ファイルの先頭に役割を説明するコメントを追加することが含まれます。指示ファイルでこれらを Copilot に伝えます。

### カスタム指示

カスタム指示は、コーディングスタイル、要件、使用ライブラリなどのコンテキストを Copilot に伝えます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストに送られるプロジェクト全体向けの指示です。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに指針を適用します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。Copilot Chat、Copilot CLI、Copilot cloud agent は `applyTo` frontmatter を持つ指示ファイルもコード生成時に使います。AGENTS.md や CLAUDE.md などの形式にも対応しています。

指示ファイルでは、プロジェクト全体の指針を `copilot-instructions.md` にまとめ、特定のファイル種別やタスクの内容を `*.instructions.md` に記述します。明確な自然言語とコード例を使います。

> [!TIP]
> プロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions] 向けの指示があります。Copilot に生成を依頼することもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境で手順が異なります。例は [awesome-copilot][awesome-copilot]で確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの指示ファイルを確認する

1. `.github/copilot-instructions.md` を開きます。
2. **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などの内容を確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も読みます。
3. `.github/instructions` を開き、Astro、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` はリポジトリルートからの glob を指定し、たとえば `**/*.test.ts` に一致するファイルに指示を適用します。
5. 単体テストの作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾で、他の指示ファイルやプロジェクト例へのリンクを確認します。パスは指示ファイルからの相対パスです。

> [!NOTE]
> `.github/copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントの規則がありません。次に TSDoc コメントとファイルヘッダーを追加します。

## 変更用ブランチを作成する

前の演習の Codespace に戻り、今回の変更用にブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) でターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

この後の操作を始める前に、Codespace で Copilot Chat パネルを開き、**New Chat** を選んで新しい会話を始めます。モードとモデルの選択は演習によって異なります。

1. 前の演習のエディタータブを閉じ、必要なコンテキストだけを残します。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント選択メニューから **Agent** を選びます。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認し、ヘルパーとテストなどのコードを変更する様子を見ます。
6. 提案されたヘルパーが型付きの関数で、最初の引数に `db` クライアントを受け取り、発行元の型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の規約によるものです。
7. 提案されたコードに TSDoc コメントとファイルレベルのコメントヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもコメントを追加する場合があります。その場合も、指示更新後の一貫性向上がこの演習のポイントです。

## リポジトリ標準を追加する

Copilot のプロジェクト全体向け指示に、コードドキュメントの標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近の **Code formatting requirements** セクションを探し、コード内ドキュメントの規則がまだないことを確認します。
3. 既存の標準の直後に、次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

## プロンプトを再実行して変化を見る

指示を更新した状態で同じプロンプトを再実行し、生成結果を比較します。

1. Copilot Chat に戻り、**New Chat** を選んで会話をクリアします。
2. `src/lib/publishers.ts` を選択します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しい標準が Copilot の提案に反映されました。次の演習で使うコードにも適用します。

## 指示とブランチをコミットしてプッシュする

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドで指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示ファイルと Copilot がヘルパー用に変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## PR を作成してマージする

GitHub ツールを使い、現在のブランチから PR を作成してバックログの指示標準 Issue に関連付け、マージを依頼します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** を選びます。
3. 次のプロンプトを送信します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. 必要に応じて Copilot が情報を収集し操作を実行するための **Allow** を選択します。VS Code から GitHub へのサインインを求められた場合は、画面の案内に従います。
5. PR が作成されたら、次のプロンプトでマージしてローカルの `main` を更新します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作とシェルコマンドの実行許可を求められたら、内容を確認して **Allow** を選択します。

## まとめと次のステップ

この演習では、既存指示のもとでコード生成を依頼し、リポジトリ全体の標準を追加して同じプロンプトを再実行し、提案の変化を確認しました。指示とヘルパーを `main` に反映したので、次は[エージェントモード][next-lesson]でフィルター機能を追加します。

## リソース

- [GitHub Copilot のカスタム指示][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[awesome-copilot]: https://github.com/github/awesome-copilot
