# RSpec Bulk Insert

## 概要

テスト書く時に大量のデータが必要だが、愚直に `create_list` で用意するとパフォーマンスが悪くなる。これは作成件数分のクエリが発行される + バリデーションが走るため。

回避策として、バルクインサートを用いると良いっぽい。 ActiveRecord と Mongoid のパターンをここでは示しておく。

### ActiveRecord の場合

rails >= 6.0.0 以降で利用可能

```rb
users = build_list(:users, 100)
User.insert_all(users.map(&:attributes))
```

### mongoid の場合

Mongoid >= 5.0 以降で利用可能

```rb
users = build_list(:users, 100)
User.collection.insert_many(users.map(&:attributes))
```

## 参考

- [insert_all | Railsドキュメント](https://railsdoc.com/page/insert_all)
- [Upgrading to Mongoid 5 / mongo-ruby-driver](https://blog.appsignal.com/2016/03/21/upgrading-mongoid.html)
