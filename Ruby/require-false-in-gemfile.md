# require: false in Gemfile

Gemfile にある以下のような記述が何をやってるのか知らなかったのでメモ

```Gemfile
gem 'gem_name', require: false
```

`false` を指定すると、アプリケーション全体で自動でその gem を読み込まなくなる。そのため、その gem が必要なファイル内で都度宣言が必要となる。

```Gemfile
gem 'gem_file'
```

`false` を指定しなかった場合はファイルごとに `require` を宣言せずとも利用できる。

## 参考

- [Bundler: gemfile](https://bundler.io/v2.6/man/gemfile.5.html#REQUIRE-AS)