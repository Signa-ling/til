# Autobuild

Mongoid でのモデルの関連付けに関するオプションの1種

ドキュメントをインスタンス化する際、それに関連付いた別コレクションのドキュメントを自動的にインスタンス化する。

```rb
class Band
  include Mongoid::Document

  embeds_one :label, autobuild: true # こんな感じで指定
  has_one :producer, autobuild: true # embeds_one, has_one に対して指定できる
end

band = Band.new
band.label # 存在する
band.producer # 存在する
```

なお、`embeds_many` や `has_many` では使えない。

また、あくまでもインスタンス化だから関連付いたドキュメントの保存まではされてないみたい。

## 参考

- [関連付け - Mongoid v9.0 - MongoDBドキュメント](https://www.mongodb.com/ja-jp/docs/mongoid/current/data-modeling/associations/#autobuild)
