# after_initialize

Active Record のコールバックの一種らしいが、知らなかったので。

以下のいずれかの条件に該当すると発火する。

1. Active Record Object が `new` で直接インスタンス化
2. レコードが DB から読み込まれる

コードで示すと以下のようなイメージ

```rb
class Sample < ApplicationRecord
  after_initialize :say_hello

  private

  def say_hello
    puts('hello')
  end
end

Sample.new # 発火
Sample.first # 発火, find とかでも同様
```

もし `new` された時だけ呼ぶようにしたいなどがある場合は実行条件を付与してあげる必要がある

```rb
class Sample < ApplicationRecord
  after_initialize :say_hello, if: :new_record? # 新規の場合のみ実行される

  # 中略
end
```

## 参考

- [Active Record コールバック - Railsガイド](https://railsguides.jp/active_record_callbacks.html#after-initialize%E3%81%A8after-find)
