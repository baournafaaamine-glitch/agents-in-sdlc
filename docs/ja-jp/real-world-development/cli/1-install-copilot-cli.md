---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、プロジェクトを確認して、登録済みのフィルター Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールを利用できます。このレッスンでは、インストールと認証を済ませ、ワークショップで使える状態にします。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話で動作を確認します。
- 組み込みの GitHub MCP サーバーを使ってフィルター Issue を探します。

## シナリオ

チームは、増え続けるバックログに対応するため AI エージェントを使い始めています。Copilot CLI を使えば、開発者が日常的に作業するターミナルからエージェントを利用できます。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js がインストール済みなので、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js が必要なバージョンを満たすことを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI から GitHub アカウントの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められたら、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. 次の確認が表示されたら、パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

> [!NOTE]
> Codespace では GitHub セッションですでに認証されている場合があります。認証を求められずに Copilot CLI が起動すれば、そのまま続行できます。

## 基本操作を確認する

通常のシェルプロンプトではコマンドが Codespace で実行されます。Copilot CLI の起動後は、自然言語でエージェントに依頼し、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> で確定します。
2. `/help` を入力して使用中のバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> で閉じます。
3. 次の質問をして動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが使えることを確認します。
6. 次の依頼でフィルター Issue を検索します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の変更を行った後、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチ上で動作し、自動的に worktree を作成することはありません。各変更の前に機能ブランチを作成します。

## ワークショップ用の起動オプションを使う

Copilot CLI は、許可された操作以外のツールを使う前に確認を求めます。このワークショップでは Codespace 内の承認プロンプトを省くため、`--yolo` を使って再起動します。

> [!CAUTION]
> `--yolo` は `--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を有効にします。Codespace や VM などの隔離された環境でのみ使い、日常の開発で既定の alias にしないでください。詳細は[ツールの使用を許可または拒否する方法][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` も使い、後のレッスンで Issue やプルリクエストを操作する GitHub MCP の読み書きツールを有効にします。Codespace によりローカルコンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから次のコマンドで再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. もう一度簡単な質問をして会話を確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後のレッスンで指示を変更したりエージェントを追加したりした後は、`copilot --resume` で同じ会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーでフィルター Issue を見つけました。

次は、[最初の小さな変更][next-lesson]として、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
