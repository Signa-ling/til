# Create Pull Request template

1. `./github` ディレクトリ内に `pull_request_template.md`（`PULL_REQUEST_TEMPLATE.md` でも可）を用意
2. 用意した markdown の中身をよしなに書き換える
3. main ブランチに変更を反映しておく

markdown の例

```md
# 関連 issue

# 背景

- なぜやるのかの理由を書いておく

# やったこと

- どういうことをしたかを書く

# 影響範囲

- どの辺に影響するか記載

# 動作確認

- スクリーンショットとか動画とか貼る（Before, After が分かると良い）
- 確認可能な画面のリンクなどあればそれも貼っておくと良い

# その他
```

上記3ステップ完了後に PR を作成すると、2 で記載した内容があらかじめ表示されるようになる

## 参考

- [リポジトリ用のプルリクエストテンプレートの作成](https://docs.github.com/ja/communities/using-templates-to-encourage-useful-issues-and-pull-requests/creating-a-pull-request-template-for-your-repository)
