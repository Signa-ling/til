# asdf からの移行

## 概要

asdf を元々使っていたが、

1. 昨今の開発ツールのバージョン管理ツールの主流が mise になりつつある
2. asdf 0.16.0 以降はインストールフローが変化 + インストールフローがやや複雑な印象
3. mise は asdf と互換性がある ( `.tool-versions` が使える )

といった理由から乗り換えることにした。

自分の環境は zsh なので以下の5ステップで移行を行った

```sh
# 1. mise install
curl https://mise.run | sh

# 2. ~/zshrc で mise の activate
echo 'eval "$(~/.local/bin/mise activate zsh)"' >> ~/.zshrc

# 3. ~/zshrc で asdf の shims の記載を消す
export PATH="${ASDF_DATA_DIR:-$HOME/.asdf}/shims:$PATH" # ← 削除

# 4. ターミナル再起動

# 5. asdf の削除（任意）
rm -rf ~/.asdf
sudo rm /usr/local/bin/asdf
```

一応これで移行は完了するが、動作確認するなら適当なディレクトリ配下で何かしらツールを入れてみると良い

```sh
mise use node@24.3.0
```

これでそのディレクトリ配下に `mise.toml` が作成されるなら問題なし

## 備考

`.tool-versions` での互換性があるとのことだが、基本的に `mise.toml` で管理されるらしい
もし `.tool-versions` にファイルを追加していきたいなら `.tool-versions` を直にいじるか、

```sh
mise use -p .tool-versions node@24.3.0
```

とするとよい

## 参考

- [Getting Started | mise-en-place](https://mise.jdx.dev/getting-started.html)
