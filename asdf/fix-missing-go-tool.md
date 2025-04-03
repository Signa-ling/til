# Fix missing go tool.md

## 背景

- asdf で go をインストールした
- その後 go のツールをインストールしたが、path が通ってなくて「そんなものないぞ」と怒られた

```bash
$ asdf plugin add golang https://github.com/asdf-community/asdf-golang.git
$ asdf install golan latest
$ asdf set golang 1.24.2 # これ書いてる時に最新版
$ go install github.com/upstash/upstash-redis-dump@latest # ツール例（今回遭遇したきっかけのツールがこれだったので記載）
$ upstash-redis-dump --help # ここでエラーになる
```

## 解決策

パスが通ってないだけなので、通せば解決する

自分の環境は zsh だったので `.zshrc` に以下を記載

```bash
# go env GOPATH としておけばプロジェクト間などで違うバージョンの go を使ってても関係なく扱えるらしい
export PATH="$(go env GOPATH)/bin:$PATH"
```

## 参考

- [go install で落とした binary に PATH を通す](https://okkun-sh.hatenablog.com/entry/2023/06/16/013008)
