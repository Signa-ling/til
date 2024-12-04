# hint field

Mongo のオプティマイザが選択した index ではなく、こちらで指定した index を使用するよう強制するフィールド

 `hint(index)`の形式で記述することで、その index を使用することを強制できる

```javascript
db.collection.hint({ key: 1 }).aggregate([
  {
    pipeline: 'example'
  }
]);
```

mongoid の場合は以下のように書くと利用できる。

```ruby
Model.collection.aggregate(pipeline, { hint: { key: 1 } })
```

基本的にはオプティマイザの方針に従う方が良いため、奥の手とも言える。

また、貼られていない index を指定するとエラーが出るため気をつけなければいけない。

mongoid + rails なら以下のようなテストを書いても良いかも。

```ruby
context 'hint に渡した index が存在する場合' do
  before do
    Model.create_indexes # index を貼っておく
  end

  it '処理が行われること' {}
end

context 'hint に渡した index が存在しない場合' do
  it 'エラーになること' {}
end
```

## 備考

自分が今回遭遇したケースだと、 想定した index を使ってくれなかったため hint で強制することになった。

可能であればなぜ使用されないのかの原因を調べることが望ましいが、原因が分からなかったためこの方針をとった。

推測でしかないが、以下の可能性を挙げておく

- キャッシュを持っていることで、過去に呼ばれた index を使い回している
  - 関係ありそうな話: [プランキャッシュ](https://www.mongodb.com/ja-jp/docs/manual/core/query-plans/)
- index 統計が古い
  - [$indexStats](https://www.mongodb.com/ja-jp/docs/manual/reference/operator/aggregation/indexStats/)
- コレクションにたくさんインデックスが設定されていることで、正しく選択ができない
  - [この記事](https://zenn.dev/akiko_pusu/articles/20211028-mongodb-m201-chap3#forcing-indexes-with-hint-(%E5%8B%95%E7%94%BB))の「hint() を使うようなケースは、コレクションにたくさんインデックスが設定されているようなケース。」より逆説的に
  - これは関係ありそうなソースなどが無いため不明

## 参考

- [集計 - MongoDB マニュアル v8.0](https://www.mongodb.com/ja-jp/docs/manual/reference/command/aggregate/#hint-an-index)
- [クラス: mongo::コレクション - ドキュメント0.9.28](https://www.mongodb.com/ja-jp/docs/ruby-driver/current/api/Mongo/Collection.html#aggregate-instance_method)
