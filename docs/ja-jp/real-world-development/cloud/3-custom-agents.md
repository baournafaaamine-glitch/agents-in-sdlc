---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` に Markdown ファイルを作成し、特定の作業を効果的に進めるための指示、ベストプラクティス、コーディングパターン、専門知識を定義します。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを支援するセキュリティエージェント、一貫したテストを保つテストエージェントを作成できます。

カスタムエージェントはプロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。ファイルには少なくとも `name` と `description` の YAML frontmatter を記述し、その後に動作、専門性、指示を Markdown で記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。大まかには、**カスタムエージェントが作業者、スキルがツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを調整します。このラボのアクセシビリティカスタムエージェントは、アクセシビリティ指針に基づいてサイトをレビューし、更新します。その際、プルリクエストワークフローやテスト実行などのスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントの定義方法と、タスクの割り当て方を確認します。

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべての利用者がクラウドファンディングサイトを使えるようにしたいと考えています。現在のダークテーマは、テキストと背景のコントラストが不十分で読みにくいというフィードバックがありました。デザインチームは、利用者が切り替えられるハイコントラストモードを求めています。Web Content Accessibility Guidelines (WCAG) への適合は重要なため、カスタムエージェントを使って機能を実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。指示内容を確認します。

1. Codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` の frontmatter はカスタムエージェントに必須です。

3. 続くセクションを読み、アクセシブルなサイトのコード生成に関する責任、ベストプラクティス、HTML/CSS/JavaScript の例、よくある問題を確認します。

## Issue を作成して割り当てる

Mission control では、Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を送れます。まずハイコントラストモードの Issue を Copilot に割り当てます。

1. リポジトリを開き、**Issues** タブを選択します。
2. **New issue**、**Blank issue** の順に選択します。
3. **Title** に `Add high contrast mode to website` と入力します。
4. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. **Create** を選択して Issue を作成します。
6. 右側の **Assign to Copilot** を選択します。
7. 一覧から **Accessibility agent** を選び、**Assign** を選択します。

   ![カスタムエージェントとアクセシビリティエージェントが選択された cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

Copilot はバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、GitHub Copilot のカスタムエージェントを確認し、アクセシビリティ機能に特化した役割を使いました。次は、[mission control からエージェントのセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
