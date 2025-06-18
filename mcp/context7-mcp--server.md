# Context7 MCP Server

Contex 7 MCP Server はドキュメントの最新情報を AI に提供することを目的に開発された MCP Server である。

エージェント型コーディングツールなどに用いられるモデルは、ある時期までの情報をもとに学習されている。
そのため、ある言語やライブラリなどにおいて、古い書き方を提案してしまうことが発生する。

この MCP を使うことで、新しい情報を高い信頼性のもと取得し、その情報から AI はより質の高い提案を行えるようになる…らしい。

## 導入方法

ツールによってどこに、どのような内容を書くかは異なっている。（これは MCP に関する実装がツールによって異なるせい）

例えば Cursor なら `mcp.json` に以下のような記載をすれば良い。


リモートサーバーに繋ぐ場合

```json
{
  "mcpServers": {
    "context7": {
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

ローカルサーバーに繋ぐ場合

```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "@upstash/context7-mcp"]
    }
  }
}
```

その他は公式リポジトリを参考に。

## 使い方

プロンプトに `use context7` という記述をすれば OK

```md
React で Todo アプリを作ってください。 use context7
```

## 参考

- [upstash/context7: Context7 MCP Server -- Up-to-date code documentation for LLMs and AI code editors](https://github.com/upstash/context7)
