---
title: "応用テクニック"
---

# 応用テクニック

## MCP サーバーの活用

MCP（Model Context Protocol）は、Claude Code に外部ツールやデータソースを接続するためのプロトコルです。MCP サーバーを追加することで、Claude Code の能力を大幅に拡張できます。

### MCP サーバーの設定

プロジェクトの `.claude/settings.json` で MCP サーバーを設定します。

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@context7/mcp"]
    },
    "playwright": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/mcp-playwright"]
    }
  }
}
```

### 便利な MCP サーバー

| サーバー | 用途 |
|---|---|
| Playwright | ブラウザ操作、E2Eテスト |
| Context7 | ライブラリドキュメントの参照 |
| PostgreSQL | データベースの直接操作 |
| GitHub | Issue・PR の管理 |

### 実用例: ドキュメント参照

Context7 MCP サーバーを使うと、ライブラリの最新ドキュメントを参照しながらコーディングできます。

```
> Next.js の App Router でミドルウェアを作りたい。
> 最新のドキュメントを確認して実装して。
```

Claude Code は MCP 経由でドキュメントを取得し、最新の API に基づいたコードを生成します。

## フック機能

フックは、Claude Code の特定のアクションの前後にシェルコマンドを実行する機能です。

### フックの設定

`.claude/settings.json` にフックを定義します。

```json
{
  "hooks": {
    "preCommit": {
      "command": "npm run lint && npm test"
    },
    "postFileEdit": {
      "command": "npx prettier --write $FILE"
    }
  }
}
```

### 活用例

**自動フォーマット**: ファイル編集後に自動で Prettier を実行

```json
{
  "hooks": {
    "postFileEdit": {
      "command": "npx prettier --write $FILE"
    }
  }
}
```

**コミット前チェック**: コミット前に lint とテストを自動実行

```json
{
  "hooks": {
    "preCommit": {
      "command": "npm run lint && npm run test:affected"
    }
  }
}
```

## 権限設定の最適化

プロジェクトの要件に応じて、Claude Code の権限を細かく制御できます。

### 許可リストの設定

```json
{
  "permissions": {
    "allow": [
      "Read(**)",
      "Edit(src/**)",
      "Bash(npm test)",
      "Bash(npm run lint)",
      "Bash(git *)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Edit(.env*)"
    ]
  }
}
```

この設定により、`.env` ファイルの編集を禁止しつつ、テストや lint の実行は自動で許可できます。

## CI/CD との連携

Claude Code を CI/CD パイプラインに組み込むことで、自動化をさらに進められます。

### GitHub Actions での活用

```yaml
name: Claude Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      - name: Run review
        run: |
          claude -p "この PR の変更をレビューして、
          問題があれば指摘して" --output-format json
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

### 自動テスト生成

PR が作成されたタイミングで、変更されたファイルのテストを自動生成できます。

```yaml
- name: Generate tests
  run: |
    claude -p "変更されたファイルに対するテストを生成して。
    既存のテストパターンに合わせて。" \
    --allowedTools "Read,Write,Bash(npm test)"
```

## カスタム出力フォーマット

Claude Code の出力を JSON 形式で取得し、他のツールとパイプラインでつなげます。

```bash
# JSON 出力
claude -p "src/ のTODOコメントを一覧にして" --output-format json

# 結果をファイルに保存
claude -p "テスト結果を分析して" --output-format json > report.json
```

これらの応用テクニックを組み合わせることで、開発ワークフロー全体を効率化できます。
