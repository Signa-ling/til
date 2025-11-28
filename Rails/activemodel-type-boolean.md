# ActiveModel::Type::Boolean の変換ルール

## 概要

Model や FormObject などで

```rb
attribute :hoge, :boolean
```

みたいに書くことがある。

このフィールドに値を放り込もうとすると自動で `ActiveModel::Type::Boolean` の変換ルールに基づいて変換される

ルールは大きく3つ

1. `[ false, 0, "0", :"0", "f", :f, "F", :F, "false", :false, "FALSE", :FALSE, "off", :off, "OFF", :OFF, ]` のいずれかに該当する場合は `false` とみなす
2. `nil`, `""` は `nil` とみなす
3. それ以外は `true` とみなす

なので、以下のようなケースでのバリデーションをかける時には注意が必要

1. パスカルケース `False` を `false` とみなしたい
2. `invalid` な値は `false` とみなしたい
3. `true`, `false` 以外は `true` としたい（表記揺れを防止したい）

## 参考

- [ActiveRecord::Type::Boolean](https://api.rubyonrails.org/classes/ActiveModel/Type/Boolean.html)
