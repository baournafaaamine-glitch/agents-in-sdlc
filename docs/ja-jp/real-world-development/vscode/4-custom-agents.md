---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルで、指示、ベストプラクティス、コーディングパターン、専門知識を定義します。たとえば [WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを支援するエージェント、一貫したテストを保つエージェントを作れます。

各エージェントはプロジェクトの `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。ファイルには少なくとも `name` と `description` の YAML frontmatter を記述し、その後に動作、専門性、指示を記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントは「作業者」、[エージェントスキル][agent-skills-concept]は「ツール」と考えると分かりやすくなります。エージェントは独自のコンテキストウィンドウを持ち、作業の中でスキルやほかのエージェントを調整できます。このラボのアクセシビリティエージェントは、アクセシビリティ指針に基づいてサイトを確認・更新し、必要に応じて PR ワークフローやテスト管理のスキルを呼び出せます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず誰でも利用できるクラウドファンディングサイトを目指しています。現在のダークテーマは文字と背景のコントラストが足りず、読みにくいというフィードバックがあります。デザインチームは、利用者が切り替えられるハイコントラストモードを求めています。カスタムエージェントを使って機能を実装します。

この演習では、既存のアクセシビリティエージェントを確認し、Copilot Chat で使ってハイコントラストモードを作成します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントが用意されています。内容を読み、Copilot への指示を確認します。

1. Codespace に戻り、ターミナルで `main` からアクセシビリティ用のブランチを作成します。Exercise 3 のフィルター PR とは分けて作業します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` の frontmatter はカスタムエージェントに必須です。

4. アクセシブルなサイト向けの責務、ベストプラクティス、HTML/CSS/JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code は `.github/agents` にあるカスタムエージェントを、Copilot Chat ビュー下部のエージェント選択メニューに表示します。

1. Codespace で Copilot Chat パネルを開き、**New Chat** を選択します。
2. Chat ビューのエージェント選択メニューで **Agent** を選びます。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

3. チャットビュー下部のエージェント選択メニューを開き、**Accessibility agent** を選びます。
4. 次の依頼をアクセシビリティエージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案して適用する様子を確認します。編集はファイルパスと差分付きでチャットに表示されます。
6. **Files changed** の表示で、エージェントが変更したファイルを確認します。

> [!NOTE]
> 処理には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを変更します。

## まとめと次のステップ

このレッスンでは、カスタムエージェントの定義を確認し、Copilot Chat のエージェントモードで使いました。次は[エージェントの作業を監視して方向付け][next-lesson]、同じセッションにライトモードを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
