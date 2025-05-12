# Enumerable#grep and #grep_v

`grep_v` を知らなくて調べた（ついでに grep も合わせて調べ直した）

どちらも `Enumerable` の提供するメソッド

## `grep`

パターンにマッチする要素を全て含んだ配列を返す

```rb
(1..10).grep(2..5)   # => [2, 3, 4, 5]
```

## `grep_v`

パターンにマッチしない要素を全て含んだ配列を返す

```rb
(1..10).grep_v(2..5)   # => [1, 6, 7, 8, 9, 10]
```

ちなみに関数名の `v` は Unix コマンドの `grep -v` が由来っぽい。

```sh
$ man grep

  -v, --invert-match
          Selected lines are those not matching any of the specified patterns.
```

## 参考

- [Enumerable#grep (Ruby 3.4 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/grep.html)
- [Enumerable#grep_v (Ruby 3.4 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Enumerable/i/grep_v.html)
- [Feature #11049: Enumerable#grep_v (inversed grep) - Ruby - Ruby Issue Tracking System](https://bugs.ruby-lang.org/issues/11049)
