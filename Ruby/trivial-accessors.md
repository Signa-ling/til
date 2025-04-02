# TrivialAccessors

Rubocop の警告の一つ

`attr_*`のようなアクセサ(e.g. `attr_reader`)で記載できる部分を自分で定義していると発生する

```ruby
class Sample
  # 以下は全て NG パターン

  # attr_reader :foo と書くことで代用できる
  def foo
    @foo
  end

  # attr_writer :bar と書くことで代用できる
  def bar=(val)
    @bar = val
  end
end
```

また以下のオプションがある

| オプション名 | デフォルト値 | 説明 |
|------------|------------|------|
| `ExactNameMatch` | `true` | `true` の場合、メソッド名とインスタンス変数名が完全一致する場合のみ警告を出す |
| `AllowPredicates` | `false` | `true` の場合、述語メソッド（`?` で終わるメソッド）は検査対象から除外される |
| `AllowDSLWriters` | `false` | `true` の場合、DSL 風の書き方をするライブラリ向けの setter メソッドを許可する |
| `IgnoreClassMethods` | `false` | `true` の場合、クラスメソッドには警告を出さない |
| `AllowedMethods` | `[]` | 指定したメソッド名は、チェック対象から除外する |

### `ExactNameMatch`

```rb
def name
  @other_name # false の場合に警告が出る
end
```

### `AllowPredicates`

```rb
def foo?
  @foo # false の場合は attr_* を使うよう警告が出る
end
```

### `AllowDSLWriters`

```rb
def on_exception(action)
  @on_exception=action # false の場合はattr_* を使うよう警告が出る
end
```

### `IgnoreClassMethods`

```rb
def self.foo
  @foo # true の場合はこの書き方で良い
end

class << self
  attr_reader :foo # false の場合はこう書く必要がある
end
```

### `AllowedMethods`

```rb
# AllowedMethods: ['allowed_method'] と書いてある場合、下記は警告がでない
def allowed_method
  @foo
end
```

## 参考

- [Class: RuboCop::Cop::Style::TrivialAccessors — Documentation for rubocop (1.75.1)](https://www.rubydoc.info/gems/rubocop/RuboCop/Cop/Style/TrivialAccessors)
