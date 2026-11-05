---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** を使うと、使い慣れたコードエディターで GitHub Copilot を利用できます。Visual Studio Code と GitHub Codespaces で Copilot Chat のエージェントモードを使い、Model Context Protocol (MCP) で外部ツールを接続し、カスタムエージェントを利用します。統合開発環境 (IDE) を離れずに作業でき、Copilot はファイル、ターミナル、問題の全体を把握できます。

まずカスタム指示を追加し、Copilot がその指示に従う様子を確認します。次にエージェントモードで UI、データ層、テストにまたがるフィルター機能を構築します。続いて Playwright MCP サーバーを接続し、PR 作成前に Copilot にブラウザーで機能をテストさせます。最後にアクセシビリティ用のカスタムエージェントを確認して使い、Copilot の変更を監視して方向付けしながら改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | VS Code でカスタム指示を追加して検証します |
| [2. エージェントモード][ex2] | コード生成 | エージェントモードでフィルター機能を構築します |
| [3. Playwright MCP による確認][ex3] | 外部ツール | Playwright MCP サーバーでブラウザーから機能をテストします |
| [4. カスタムエージェント][ex4] | 特化型エージェント | カスタムエージェントを確認して使います |
| [5. エージェントの管理][ex5] | 監視 | エージェントセッションを監視して指示を出します |
| [6. 反復して改善する][ex6] | レビュー | Copilot の作業をローカルで確認し、次の手順を選びます |
| [任意: Foundry を組み込む][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit でモデルを準備し、エージェントをデプロイしてサイトに接続します |

## 前提条件

ワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランが設定された GitHub アカウント
- [ ] GitHub Codespaces へのアクセス

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

## はじめる

**[演習 0「前提条件」から始めます →][ex0]**

[callout-student-plan-education]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
