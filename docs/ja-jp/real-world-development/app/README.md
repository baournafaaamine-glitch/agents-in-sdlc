---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップアプリケーションで、エージェント主導の開発を 1 つの作業用ワークスペースで実現します。エージェントセッションの並列実行、切り替え可能なセッションモード、共有キャンバス、GitHub Issue とプルリクエストのネイティブ管理に対応しています。また、リベース、レビューコメント、継続的インテグレーション (CI) の修正、マージまで PR を支援する **Agent Merge** も備えています。

このワークショップでは、Tailspin Toys の一連のワークフローを進めます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリに接続し、ワークスペースと登録済みバックログを確認します。
2. 星評価の小さな変更を行い、ブラウザーで確認し、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター Issue から始め、**Plan** モードで方針を決め、**Autopilot** モードで構築し、**Interactive** モードでレビューします。
4. リポジトリの指示を更新して、フィルター作業に適用します。
5. 既存の `quality-checks` スキルを調整し、プロジェクトのチェックに使います。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルターを確認します。
7. 品質保証 (QA) カスタムエージェントを作成し、要件、カバレッジ、検証の根拠をレビューします。
8. フィルター変更全体をレビューし、2 つ目の PR に Agent Merge を使います。
9. キャンバス機能が利用できるか確認して Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストします。必要な機能が使えない場合は、ブロッカーを記録し、読み取り専用の代替手段を使うか、まとめに進みます。

ワークショップを簡潔にするため、PR は 2 つ作成します。星評価と、指示、スキル、QA プロファイル、テストの更新を含むフィルター機能です。それぞれ更新済みの `main` から開始します。フィルターと品質ワークフローでは 1 つのセッション、worktree、ブランチを共有して、各ツールの操作を通して作業を積み重ねます。最後のキャンバス演習も同じセッションで行い、PR ワークフローを繰り返さず、共有サーフェスの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトのコピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトに接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 小さな成果][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、1 つ目の PR をマージします |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認・更新し、フィルター機能に適用します |
| [5. quality-checks スキルを調整して使う][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize から MCP を設定し、フィルターの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門プロファイルを作成して選び、最終検証の根拠を集めます |
| [8. 機能 PR の作成とマージ][ex8] | レビューとマージ | フィルター、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、参考資料を振り返ります |

## 前提条件

ワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランが設定された GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。これは Copilot CLI のポリシーとは別です。

## はじめる

**[前提条件から始めます →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
