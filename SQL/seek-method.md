# Seek method

一旦こういう手法があるということでメモ。どこかで自分でクエリを書いて確認してみたい。

ページネーションを実現する時に `limit` と `offset` を組み合わせることが多いが、これは後ろのページを開こうとするほど遅くなる。

別案としてシーク法と呼ばれるやり方があるらしい。

これは WHERE 条件に主キーを与えることで高速にページ検索を行うもの。

```sql
SELECT * FROM USERS
WHERE id > 10 -- 前ページ最後のレコードの user.id
ORDER BY id
LIMIT 10
```

難点として、ソート条件が複雑になると実装もその分複雑になることや、任意のページにジャンプする際に工夫が必要とのこと。

## 参考
- [OFFSETは前の行を読み飛ばすのにはよくない方法](https://use-the-index-luke.com/ja/sql/partial-results/fetch-next-page)
- [offsetでページネーションは遅い。これからはシーク法だ！ #MySQL - Qiita](https://qiita.com/madilloar/items/b4e786a932ef9d4551b9)
- [SQLのページネーションでOFFSETを使うとレコードが重複する](https://raahii.me/posts/sql-paging-orderby/)
