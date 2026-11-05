---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連の作業で GitHub Copilot CLI を使いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター Issue を見つけました。
- 星評価を追加し、転送ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルを調整して単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキル結果、ブラウザーでの根拠を評価しました。
- フィルター変更全体をレビューし、Agent Merge によるマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作り、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートを調整し、QA プロファイルと関連テストを追加します。

フィルターの計画から PR 作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## 検証方法の違い

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行して、新しい形式で報告します。QA エージェントは、要件とテストカバレッジのレビューをこれらの結果とまとめます。

追加テストは、実際のカバレッジ不足を補うものである必要があります。QA の実行で追加テストが不要と判断される場合もあります。マージを許可する前に、コードと根拠を確認し、変更後は影響する検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが作業に影響します。このワークショップでは、指示を更新し、スキルを調整し、QA プロファイルを作成し、MCP サーバーを設定しました。指示は標準を定め、スキルは繰り返し可能なタスクを記述し、カスタムエージェントは専門役を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前の検討には **Plan**、範囲を絞った変更を対話しながら進めるには **Interactive**、明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に伝えると、出力が大きく変わります。

## さらに試す

ほかにも次の CLI 機能があります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別タスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

ツールの使い方を身に付ける最善の方法は、継続して使うことです。製品コード、個人のコード、以前から作ろうと思っていた小さなアプリなどで試してください。学んだことをチームで共有し、チームの経験からも学びましょう。引き続きドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境での作業を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
