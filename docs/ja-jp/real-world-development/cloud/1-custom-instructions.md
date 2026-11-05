---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、タスクの進め方や Copilot が知っておくべき背景情報を参照できるようにすることが重要です。[指示ファイル][instruction-files]を使うと、何を行うかだけでなく、どのように行うかも伝えられます。

この演習では、リポジトリのカスタム指示とパス指定の指示ファイルを確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスと異なり、ここでは指示追加の前後でプロンプトの結果を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で、cloud agent が作成した PR をレビューするときに指示の影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発ガイドラインがあります。

- データ層には必ず単体テストを追加します。
- UI はダークモードで、現代的な印象にします。
- コードのドキュメントには TSDoc コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発方針に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイル、要件、使用ライブラリなどのコンテキストを Copilot に伝えます。リポジトリ全体または特定のファイル種別に適用できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストに送られるプロジェクト全体向けの指示です。技術スタック、プロジェクトの概要、共通の指針などを記述します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語、UI コンポーネントや単体テストなどのタスクに適用できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの形式の指示ファイルにも対応しています。

指示を管理する際は、リポジトリ全体に適用する指針を `copilot-instructions.md` にまとめ、特定のファイル種別やタスクの指示は `*.instructions.md` に分けます。自然な言葉で明確に書き、コードの良い例と避ける例を示します。どのプロジェクトにも当てはまる唯一の方法はないため、実際に試して改善します。

> [!TIP]
> このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] などのタスクに対応する指示ファイルがあります。Copilot に指示ファイルの生成を依頼することもできます。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。テンプレートや例は [awesome-copilot][awesome-copilot]で確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された指示ファイルを読み、適用範囲と内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指針も読みます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` がリポジトリルートからの glob で適用対象を指定していることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルに適用されます。
5. プロジェクトの単体テスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾にある他の指示ファイルやプロジェクトファイルへのリンクを確認します。パスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次に TSDoc コメントとファイルヘッダーの規則を追加します。

## リポジトリの標準を追加する

cloud agent は Issue が対象とするブランチから指示ファイルを読み込みます。次の演習では `main` からブランチを作るため、指示を `main` にマージしておきます。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き、**Code formatting requirements** セクションを探します。現在、コード内ドキュメントの規則がないことを確認します。
3. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリレベルの `.github/copilot-instructions.md` はプロジェクト全体の標準を記述する場所です。特定の言語やタスクには `*.instructions.md` を使います。

## 指示の変更をコミット、プッシュ、マージする

次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。指示を確実に適用するため、変更を `main` に反映します。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への PR を作成し、マージします。次の演習で cloud agent がこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接コミットしてもかまいません。ブランチを使う手順は、実際のプロジェクトでの作業方法を反映しています。

## まとめと次のステップ

この演習では、プロジェクト内の指示ファイルから Copilot がコンテキストを得る方法を確認し、`.github/copilot-instructions.md` に新しい標準を追加しました。cloud agent が今後作成する PR で、この標準が使われることを確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示作成のベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
