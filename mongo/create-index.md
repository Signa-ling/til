# Create index

## mongo の場合

mongoDB で index を追加するには以下のように書く。

```sh
$ mongo
> use some_db_name
> db.some_collection_name.create_index({ key_name: sort_direction })
```

`sort_direction` は昇順インデックスなら 1 、降順インデックスなら -1 を指定する（sort 時に役立つらしい）

## rails × mongoid の場合

事前に以下のような model ファイルがあるとする。

```ruby
class SampleModel
  field :hoge, type: String

  index hoge: 1
end
```

このファイルを元に追加する場合は

```sh
$ bundle exec rails c
[1] pry(main)> SampleModel.create_indexes
```

とすることで、そのモデルファイルに記載された index が書き込まれる…らしい（自分では試していない）

複数のモデルファイルの index をまとめて貼りたい場合、

```sh
$ bundle exec rake db:mongoid:create_indexes
```

とすると良い。

なお、これの実行ログは標準出力されず、rails の log ファイルに記載されている（設定次第で標準出力したりできるかもしれない）

## 参考

- [db.collection.createIndex() - MongoDB マニュアル v8.0](https://www.mongodb.com/ja-jp/docs/manual/reference/method/db.collection.createIndex/)
- [インデックス管理 - Mongoid](https://www.mongodb.com/ja-jp/docs/mongoid/current/reference/indexes/#index-management-rake-tasks)
