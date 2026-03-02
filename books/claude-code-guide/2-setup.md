---
title: "セットアップ"
---

# セットアップ

## 動作要件

Claude Code を使うには、以下の環境が必要です。

- **Node.js**: v18 以上
- **OS**: macOS、Linux、または Windows（WSL2 経由）
- **ターミナル**: Bash または Zsh
- **Anthropic アカウント**: Claude の API キーまたは Claude Pro/Max サブスクリプション

## インストール

npm を使ってグローバルにインストールします。

```bash
npm install -g @anthropic-ai/claude-code
```

インストールが完了したら、バージョンを確認しましょう。

```bash
claude --version
```

## 認証設定

初回起動時に認証が求められます。

```bash
claude
```

2つの認証方法があります。

### 方法1: Claude のサブスクリプション（推奨）

Claude Pro または Max のサブスクリプションがある場合、ブラウザ経由の OAuth 認証が使えます。起動時に表示される URL をブラウザで開き、ログインするだけで完了です。

### 方法2: API キー

Anthropic Console で API キーを発行し、環境変数に設定します。

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

永続化するには、シェルの設定ファイルに追記します。

```bash
# .bashrc または .zshrc に追加
echo 'export ANTHROPIC_API_KEY="sk-ant-..."' >> ~/.zshrc
source ~/.zshrc
```

## 初期設定

### 権限モード

Claude Code にはファイル操作やコマンド実行の権限モードがあります。初回起動時にプロンプトで設定するか、起動時にフラグで指定します。

```bash
# 許可確認なしで全操作を実行（信頼できるプロジェクトのみ）
claude --dangerously-skip-permissions

# デフォルト: 操作ごとに確認を求める
claude
```

開発中は適切な権限設定を選びましょう。個人プロジェクトであれば柔軟に、チームプロジェクトでは慎重な設定がおすすめです。

### モデルの選択

使用するモデルを指定できます。

```bash
# デフォルト（最新の Claude モデル）
claude

# モデルを明示的に指定
claude --model claude-opus-4-6
claude --model claude-sonnet-4-6
```

用途に応じてモデルを使い分けると効果的です。

| モデル | 特徴 | 向いている用途 |
|---|---|---|
| Opus | 最も高性能、深い推論 | 設計判断、複雑なリファクタリング |
| Sonnet | バランス型、高速 | 日常的なコーディング |
| Haiku | 最速、軽量 | 簡単な質問、ファイル検索 |

### 作業ディレクトリ

Claude Code はカレントディレクトリをプロジェクトルートとして認識します。必ずプロジェクトのルートディレクトリで起動しましょう。

```bash
cd ~/projects/my-app
claude
```

Git リポジトリのルートで起動すると、プロジェクトの構造やコミット履歴も参照できるため、より適切な支援が得られます。

## 動作確認

セットアップが完了したら、簡単なタスクを試してみましょう。

```bash
claude

# Claude Code が起動したら、以下のように入力
> このプロジェクトの構造を説明して
```

プロジェクトのファイル構成が表示されれば、セットアップは完了です。次章では日常的な使い方を見ていきます。
