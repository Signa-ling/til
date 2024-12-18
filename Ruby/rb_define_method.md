# rb_define_method

`Array#count` の処理を読んでたら出てきたので調べた。

以下のように記述しており、クラス `klass` のインスタンスメソッド `name` を定義しているらしい。

```c
void rb_define_method(VALUE klass, const char *name, VALUE(*func)(), int argc)
```

また、 `argc` は C の関数に渡される引数の数と形式を定めるもので、大きく以下の3つに分かれる。

|  | argc が 0 以上 | argc が -1 | argc が -2 |
| - | - | - | - |
| 挙動 | メソッドの引数の数 = argc となる<br>16個以上の引数は使えない | 引数を C の配列として第二引数に入れる<br>第1引数に配列の要素数が入る | 引数は ruby の配列に入れて渡される |
| コード | ```c<br>VALUE func(VALUE self, VALUE arg1, ... VALUE argN)<br>``` | ```c<br>VALUE func(int argc, VALUE *argv, VALUE self)<br>```<br> | ```c<br>VALUE func(VALUE self, VALUE args)<br>``` |

なので `Array#count` の定義として記述されていた

```c
rb_define_method(rb_cArray, "count", rb_ary_count, -1);
```

は、

「`Array` klass に `count` メソッドを定義する。これの中身は `rb_ary_count` に定義する。この `rb_ary_count` の第1引数に要素数、第2引数に C の配列として引数を渡す」

になる…っぽい？

なんか実態と違う気がする…このメソッドの実装も含め、また別の機会に調べた方が良さそう。

## 感想

ひとまず `rb_define_method` の第1〜3引数が理解できた。なので、 ruby の `Klass#method` の実装を追うのが少し簡単になった。

`rb_define_method` の呼び出し元の実装とかもみていけば、オレオレメソッドが作れそう。

## 参考

- [function rb_define_method (Ruby 3.3 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/function/rb_define_method.html)
