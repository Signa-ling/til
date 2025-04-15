# tally

以下のような配列をはじめ、`self` に含まれる各要素の数え上げをする際に使えるメソッド。

```rb
arr = ["a", "b", "c", "b"] # この場合各要素の出現回数は a: 1, b: 2, c: 1 である
arr.tally # => {"a"=>1, "b"=>2, "c"=>1} が返ってくる
```

`tally()` は引数に `Hash` を指定できる。これを用いることで結果を加算する `Hash` を指定できる。

```rb
result = {}
[:a, :b, :c].tally(result) # この時点では {:a=>1, :b=>1, :c=>1}
[:a, :b, :d].tally(result) # ↑の分と合わせて {:a=>2, :b=>2, :c=>1, :d=>1}
```

## 余談

この関数を知るまで以下のように愚直に書いてた。

```rb
arr.each_with_object(Hash.new(0)) do |element, result|
  result[element] += 1
end
```

## 参考

- [Enumerable#tally (Ruby 3.4 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/tally.html)
