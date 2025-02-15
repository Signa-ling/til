# Predicates by Enumerize

Enumerize ライブラリにおける `Predicates` の記述をちゃんと知らなかったのでメモ

```ruby
class User
  extend Enumerize

  enumerize :sex, in: %w(male female), predicates: true
end
```

のような形で `enumerize` に `predicates: true` と宣言することで以下のような記述が行える

```ruby
user = User.new(sex: 'male')

# 取り得る値? の書き方ができるようになる
user.male?   # => true
user.female? # => false
```

`predicates: { prefix: true }` という宣言もできる。

この場合、 `name_value?` な形式で書くことができる。

```ruby
class User
  extend Enumerize

  enumerize :sex, in: %w(male female), predicates: { prefix: true } # ここに prefix を付与
end

user = User.new(sex: 'male')

# instance.name_value? の書き方ができる
# 今回だと name が sex, value が male or female なので以下のように×
user.sex_male?
user.sex_female?
```

## 参考

- [Module: Enumerize::Predicates — Documentation for enumerize (2.8.1)](https://www.rubydoc.info/gems/enumerize/Enumerize/Predicates)