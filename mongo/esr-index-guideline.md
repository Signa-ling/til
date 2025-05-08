# ESR index guideline

- 複合インデックスの中でも効率的なものを作成する際のガイドラインのこと
- Equality, Sort, Range の順番で複合インデックスを貼ると良い

例えば以下のようなクエリがあったとする

```javascript
db.users.find(
  {
    firstName: "alice", // Equality
    age: { $gt: 20 } // Range
  }
).sort({ lastName: 1 }) // Sort
```

この時に最適なインデックスは以下のようになる

```js
// Equality, Sort, Range の順で使用されるフィールドを定義する
{ firstName: 1, lastName: 1, age: 1 }
```

## なぜこの順序にするのか？

Sort を軸に考えると分かりやすそうだった。

まず、Sort をするのであれば少ない件数である方が効率が良いため、先に Equality を実行する。となると、最初に Equality に用いられるフィールドをインデックスの最初に定義するのが望ましい。

次に MongoDB は Range で絞った結果に対してインデックスソートを実行できないという仕様を持っている。そのため、 Sort で並び替えた結果に対し Range を適用するのが適切となる。

## 備考

### Equality な条件が複数ある場合

例えば以下のような状態の場合

```js
db.users.find(
  {
    firstName: "alice", // Equality
    bloodType: "A", // Equality
    age: { $gt: 20 } // Range
  }
).sort({ lastName: 1 }) // Sort
```

インデックス内の Equality なフィールドの順序は任意で良い（ただし Sort または Range より前に配置されてる必要はある）

```js
{ firstName: 1, bloodType: 1, lastName: 1, age: 1 } // OK
{ bloodType: 1, firstName: 1, lastName: 1, age: 1 } // OK
{ firstName: 1, lastName: 1, bloodType: 1, age: 1 } // NG: Equality なインデックスが Sort より後ろにあるので
```

### 定義箇所が特殊な演算子について

- 以下は範囲演算子なので、`Range` の位置にフィールドを指定する
  - `$ne`, `$nin` などの不等価演算子
  - `$regex`
- `$in` を `.sort()` と併用する場合、 `$in` の配列要素がある閾値未満かどうかで適切なインデックスの指定箇所が変わる
  - 閾値未満の場合は `Equality` に含める
  - 閾値以上の場合は `Range` に含める
  - 閾値は MongoDB のバージョンによって異なる（8.0 の場合は 200）

## 参考

- [ESR（Equality, Sort, Range）ガイドライン - Database Manual v8.0 - MongoDB Docs](https://www.mongodb.com/ja-jp/docs/manual/tutorial/equality-sort-range-guideline/)

