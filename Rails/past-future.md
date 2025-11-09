# past? と future?

## 概要

Rails の ActiveSupport には、時間判定を行うためのメソッド `past?`, `future?` のメソッドが存在する

これらは現在の日時を基準に、判定対象が過去か未来かに応じて `true` or `false` を返す

```rb
date = 3.days_ago
date.past? # true
date.future? # false
```

なおこのメソッドは `Time`, `Date`, `DateTime` オブジェクトで使用可能で `self.class.current` を基準にする（例えば `Time` なら `Time.current` が基準になる）

## 備考

- 何かしらの有効期限判定とかに使えるかも
  - 実務では発行したトークンの期限判定に用いてた

## 参考

- [Active Support コア拡張機能 - Railsガイド](https://railsguides.jp/active_support_core_extensions.html#%E6%99%82%E9%96%93%E3%82%B3%E3%83%B3%E3%82%B9%E3%83%88%E3%83%A9%E3%82%AF%E3%82%BF)
