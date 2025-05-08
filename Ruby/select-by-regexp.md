# SelectByRegexp

rubocop の警告の1つ

`Enumerable` モジュールの提供する以下のメソッドの条件に正規表現マッチを使っている場合、 `grep` または `grep_v` への置換を促している

| 既存メソッド     | 推奨メソッド   |
| ---------- | -------- |
| `select`   | `grep`   |
| `filter`   | `grep`   |
| `find_all` | `grep`   |
| `reject`   | `grep_v` |

```rb
# match?, =~, !~ を正規表現のマッチパターンとして扱っている

# bad (select, filter, or find_all)
array.select { |x| x.match? /regexp/ }
array.select { |x| /regexp/.match?(x) }
array.select { |x| x =~ /regexp/ }
array.select { |x| /regexp/ =~ x }

# bad (reject)
array.reject { |x| x.match? /regexp/ }
array.reject { |x| /regexp/.match?(x) }
array.reject { |x| x =~ /regexp/ }
array.reject { |x| /regexp/ =~ x }

# good
array.grep(regexp) # select, filter, find_all
array.grep_v(regexp) # reject
```

## 備考

`hash.grep` は `hash.select` と同じ挙動にならないため警告されない

## 参考

- [RubyDoc.info: Class: RuboCop::Cop::Style::SelectByRegexp – Documentation for rubocop (1.75.5) – RubyDoc.info](https://www.rubydoc.info/gems/rubocop/RuboCop/Cop/Style/SelectByRegexp)
