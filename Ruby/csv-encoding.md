# CSV encoding

ずっと以下のように書いてた

```ruby
CSV.generate do |csv|
# 処理
end.encode(Encoding::UTF_8)
```

のだが、どうやら以下のようにしても書けるらしい

```ruby
CSV.generate(encoding: Encoding::UTF_8) do |csv|
# 処理
end
```

そもそも `generate` って引数とれたんだというところから学びだった。

るりまを読んでみると、 `CSV.new` と同じ引数がとれるらしく、加えて `encoding` を key に指定すると出力のエンコーディングを指定できるらしい。

## 参考

- [class CSV (Ruby 3.3 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/class/CSV.html)
