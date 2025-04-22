# Covered query

MongoDB では、クエリに必要な情報（条件と返却するフィールド）がすべてインデックスに含まれていると、 ドキュメント本体をメモリに読み込む必要がなくなる。これをカバードクエリという。

利点は以下の2つ

1. ディスク I/O が減るのでクエリが高速になる
2. メモリロードが発生しないことでメモリ効率も向上する

## 成立条件

カバードクエリは以下の全条件を満たすことで成立する

- クエリの条件フィールドがインデックスに全て含まれている
- 結果として返すフィールド（Projection）が全てインデックスに含まれている
  - デフォルトで返される `_id` フィールドもこの条件に該当する
  - よって、以下のいずれかの対応をしないとカバードクエリにならない
    - `_id` を index に含む（複合 index の場合明示する）
    - Projection フィールドで `_id: 0` を明示する
- クエリ内のいずれのフィールドも `null` でないこと
  - `"field": null` や `{ "field": { $eq: null } }` ではカバードクエリが作成されない

## カバードクエリ例

1. `users` コレクションに下記データがある

```json
{ "_id": ObjectId("..."), "name": "Alice", "age": 30, "city": "Tokyo" }
{ "_id": ObjectId("..."), "name": "Bob", "age": 25, "city": "Osaka" }
{ "_id": ObjectId("..."), "name": "Charlie", "age": 35, "city": "Tokyo" }
```

2. `{ "city": 1, "name": 1 }` という index が貼られている

上記状況のとき、以下のクエリはカバードクエリになる

```javascript
db.users.find(
  { city: "Tokyo" },         // 条件フィールド (city) はインデックスに含まれる
  { name: 1, _id: 0 }       // 返すフィールド (name) はインデックスに含まれ、_id は除外
)
```

なお、以下のクエリはカバードクエリにならない

```javascript
// 条件にインデックス外の age が含まれる
db.users.find(
  { city: "Tokyo", age: { $gt: 30 } }, // age はインデックスに含まれていない
  { name: 1, _id: 0 }
)

// 返すフィールドにインデックス外の age が含まれる
db.users.find(
  { city: "Tokyo" },
  { name: 1, age: 1, _id: 0 } // age はインデックスに含まれていない
)

// _id を除外せず、_id がインデックスに含まれていない
db.users.find(
  { city: "Tokyo" },
  { name: 1 }                 // _id がインデックスに含まれていない場合
)
```

## 確認方法

`explain` の下記点からカバードクエリかどうかを判断できる

- `FETCH` ステージの子孫では無い `IXSCAN` ステージがあること
- `executionStats.totalDocsExamined` が `0` であること
  - この数値は読み込まれたドキュメント数を指す
  - カバードクエリの場合、ドキュメント読み込みが発生しないので `0` になる

```json
// カバードクエリの場合の例
{
  queryPlanner: {
    ...
    winningPlan: {
      stage: 'PROJECT_COVERED', // ここはちょっと適当なので別の名前が入るかも。 FETCH にはならない
      ...
      inputStage: {
         stage: 'PROJECTION_DEFAULT',
      }
    }
    executionStats: {
      ...
      totalDocsExamined: 0, // ここが 0 になる
      ...
    }
  }
}
```

## 参考

- [結果説明 - データベース マニュアル v8.0 - MongoDB Docs](https://www.mongodb.com/ja-jp/docs/manual/reference/explain-results/#covered-queries)
- [説明計画結果の解釈 - データベース マニュアル v8.0 - MongoDB Docs](https://www.mongodb.com/ja-jp/docs/manual/tutorial/analyze-query-plan/)
- [ひとりMongoDB University / M201 MongoDB Performance(7)](https://zenn.dev/akiko_pusu/articles/20211104-mongodb-m201-chap4#%E3%82%AB%E3%83%90%E3%83%BC%E3%83%89%E3%82%AF%E3%82%A8%E3%83%AA%E3%81%AE%E5%8B%95%E3%81%8D)
