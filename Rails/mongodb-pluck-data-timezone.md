# Mongodb pluck data timezone

MongoDB + Rails で DB に保存されている値を `pluck` で取得したら日付関係のフィールドのタイムゾーンが思ってたのと違う形で取得された

## 前提

Model の設定は以下のような状態

- 時刻に関するフィールドを持つ
- このフィールドは `DateTime` class かつ、UTC 形式で保存される

```rb
class Sample
  # include 周りは省略

  field :notice_time, type: DateTime
end

# 保存イメージ
Sample.new(notice_time: DateTime.parse('2025-01-01 12:00:00 UTC')).save
```

## 挙動について

普通にモデルデータを取得しようとした場合、 Rails 側で設定したタイムゾーンになる

```rb
# application.rb

# この設定で Rails はタイムゾーンが JST になる
# これによって、サーバーなどが UTC であっても Rails 上では JST で扱える（はず）
config.time_zone = 'Tokyo'
```

上記設定の状態で `pluck` を使わずに取得すると `DateTime` class で JST 形式の値がとれる

```rb
sample = Sample.first
puts(sample.notice_time) # => Wed, 01 Jan 2025 21:00:00 +0900
```

これを `pluck` で取得すると `Time` class の UTC 形式で取得される

```rb
times = Sample.pluck(:notice_time)
puts(times.first) # => 2025-01-01 12:00:00 UTC
```

`pluck` は DB のフィールド値を効率的に取得するために、一部の型キャストなどをスキップしたり DB 側の保存値をそのまま使うケースがあるらしい。

今回のケースだとタイムゾーンによる変換を skip したということになる。（でも `Time` class には変換するんだなあ…）

## 参考

提示できるものがないので、もし見つけたら貼っておく。Gemini に聞きつつ実際にデータを確認して分かったことなので。
