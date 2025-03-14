# Check required files

`require` された file(gem) を確認したいときにどうすればいいのか気になって調べた。

`$LOADED_FEATURES` 変数を呼ぶことで確認できるらしい。

```sh
[1] pry(main)> $LOADED_FEATURES
```

返り値は配列なので、 `grep` メソッドを付与すれば確認したい gem が含まれているかの判別がしやすい。

```sh
[1] pry(main)> $LOADED_FEATURES.grep(/rack-mini-profiler/)
```

## 参考

- [rubygems - How do I get a list of files that have been `required` in Ruby? - Stack Overflow](https://stackoverflow.com/questions/7190015/how-do-i-get-a-list-of-files-that-have-been-required-in-ruby)
