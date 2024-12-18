# Array#count

このメソッドに引数を渡している記述があった。そもそも引数とれたんだ…となり調べた。

どうやらこのメソッドは引数をとることができ、取る場合と取らない場合で以下のように挙動が変わるようだ。

```ruby
ary = [1, 2, 4, 2.0]

# 引数を取らない場合、要素数を数える
ary.count             # => 4,

# 引数を1つ取る場合、渡した引数に一致する要素の個数を返す
ary.count(2)          # => 2

# ブロックを渡す場合、そのブロックを評価して真となる要素の個数を返す
ary.count{|x|x%2==0}  # => 3
```

ブロックもとれるのね〜

## ロジックを読む

独力で読める力が無さすぎたので、[該当箇所](https://docs.ruby-lang.org/en/3.3/Array.html#method-i-count)を ChatGPT に投げて教えてもらった。

```c
static VALUE

// argc: Ruby の count メソッドに渡された引数の個数
// *argv: それらの引数を保持する配列
// ary: count メソッドが呼ばれた対象の配列（Ruby オブジェクト）
rb_ary_count(int argc, VALUE *argv, VALUE ary)
{
    long i, n = 0;

    // 引数の数をチェックしているらしい
    if (rb_check_arity(argc, 0, 1) == 0) {
        VALUE v;

        // ブロックでない（つまり ruby 上で arr.count と書いた）場合、要素数をそのまま返す
        if (!rb_block_given_p())
            return LONG2NUM(RARRAY_LEN(ary));

        // ブロックの場合
        for (i = 0; i < RARRAY_LEN(ary); i++) {
            // 配列の各要素 v に対してブロックを rb_yield で呼び出す
            // RTEST(...) は戻り値が nil や false 以外であれば真とみなす
            // ブロックが真を返す要素の合計を rb_ary_count の返り値とする
            v = RARRAY_AREF(ary, i);
            if (RTEST(rb_yield(v))) n++;
        }
    }
    else {
        // count に渡した引数を取り出す（これに一致するかを判定したいので）
        VALUE obj = argv[0];

        // 引数がある時は、ブロックが渡されていても警告を出す
        // Ruby 仕様上、ブロックより引数でのカウントが優先されることに対応しているらしい
        if (rb_block_given_p()) {
            rb_warn("given block not used");
        }

        // 配列の各要素が obj （count に渡した引数）と等しいか判定して、等しかった要素数を rb_ary_count の返り値とする
        for (i = 0; i < RARRAY_LEN(ary); i++) {
            if (rb_equal(RARRAY_AREF(ary, i), obj)) n++;
        }
    }

    return LONG2NUM(n);
}
```

これを

```c
rb_define_method(rb_cArray, "count", rb_ary_count, -1);
```

として記述しているので ruby では `count` として呼び出せるっぽい。



## 参考

- [Array#count (Ruby 3.3 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Array/i/count.html)
- [ruby/array.c at master · ruby/ruby](https://github.com/ruby/ruby/blob/master/array.c#L6469)
