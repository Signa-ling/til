# MemoizedInstanceVariableName

Rubocop の警告の1つで、正式なクラス名は `Rubocop::Cop::Naming::MemoizedInstanceVariableName`

インスタンス変数名とメソッド名が一致しない、メモ化されたメソッドをチェックするというもの。

```ruby
# メソッド名 foo とインスタンス変数名 something とで名前が一致しないので怒られる
def foo
  @something ||= example_logic
end

# メソッド名 foo とインスタンス変数名 foo とで名前が一致するので怒られない
def foo
  @foo ||= example_logic
end
```

調べてみると、 `EnforcedStyleForLeadingUnderscores` という値をこの Cop では設定できるらしい

設定は以下の3パターン

- `disallowed`: デフォルトの設定でアンダースコア接頭詞をつけた関数名、インスタンス変数名を許可しない
- `required`: アンダースコア接頭詞をつけた関数名、インスタンス変数名のみを許可する
- `optional`: アンダースコア接頭詞の有無関係なく許可する

つまり、`disallowed` の場合は

```ruby
# これは許される
def foo
  @foo ||= example_logic
end

# 以下2つは許されない
def _foo
  @foo ||= example_logic
end

def foo
  @_foo ||= example_logic
end
```

`required` の場合は

```ruby
# これは許されない
def foo
  @foo ||= example_logic
end

# 以下2つは許される
def _foo
  @foo ||= example_logic
end

def foo
  @_foo ||= example_logic
end
```

`optional` の場合は

```ruby
# 全部許される
def foo
  @foo ||= example_logic
end

def _foo
  @foo ||= example_logic
end

def foo
  @_foo ||= example_logic
end
```

ということらしい。

## 余談

`Memorized` だと思っていたが `Memoized` であった。ドキュメントなどが脱字してるのかと思って思わず調べてしまった。


## 参考

- [Class: RuboCop::Cop::Naming::MemoizedInstanceVariableName — Documentation for bbatsov/RuboCop (master)](https://www.rubydoc.info/github/bbatsov/RuboCop/RuboCop/Cop/Naming/MemoizedInstanceVariableName)