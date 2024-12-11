# WithIndifferentAccess

Active Support による拡張機能の1つで、ハッシュキーのシンボルと文字列を同様に扱うというもの

以下のようにして key に対応した value を取り出せる

```ruby
# 例1: key を symbol で設定
sample1 = { black: '#000000' }.with_indifferent_access

sample1[:black]  # => '#000000'
sample1['black'] # => '#000000'

# 例2: key を string で設定
sample2 = { 'white': '#FFFFFF' }.with_indifferent_access

sample2[:white]  # => '#FFFFFF'
sample2['white'] # => '#FFFFFF'
```

また、以下のようにも書ける

```rb
rgb = ActiveSupport::HashWithIndifferentAccess.new

rgb[:black] = '#000000'
rgb[:black]  # => '#000000'
rgb['black'] # => '#000000'

rgb['white'] = '#FFFFFF'
rgb[:white]  # => '#FFFFFF'
rgb['white'] # => '#FFFFFF'

hash = ActiveSupport::HashWithIndifferentAccess.new(a: 1)
```

## 感想

便利だけど、個人的には hash 作成時の key の宣言方法に統一性が無くなるので多用したくない。

symbol の方がパフォーマンス的に有利なので、なるべく symbol による宣言がしたい。

## 参考

- [Active Support コア拡張機能 - Railsガイド](https://railsguides.jp/active_support_core_extensions.html#%E3%83%8F%E3%83%83%E3%82%B7%E3%83%A5%E3%82%AD%E3%83%BC%E3%81%AE%E3%82%B7%E3%83%B3%E3%83%9C%E3%83%AB%E3%81%A8%E6%96%87%E5%AD%97%E5%88%97%E3%82%92%E5%90%8C%E6%A7%98%E3%81%AB%E6%89%B1%E3%81%86%EF%BC%88indifferent-access%EF%BC%89)
- [ActiveSupport::HashWithIndifferentAccess](https://api.rubyonrails.org/classes/ActiveSupport/HashWithIndifferentAccess.html)