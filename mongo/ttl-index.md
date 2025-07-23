# TTL Index

## 概要

一定時間経過後、あるいは指定された時刻にコレクションからドキュメントを自動的に削除するためのインデックス

ログやセッション情報など、 DB 上に一定期間だけ保持しておきたいものに効果的

## 設定方法

例えば以下の例では `createdAt` が 3600 秒 ( = 1時間 ) で自動削除される

```javascript
db.collection.createIndex(
  { "createdAt": 1 },
  { expireAfterSeconds: 3600 }
)
```

mongoid の場合は以下のように書くと良いっぽい

```ruby
class SampleModel
  index({ created_at: 1 }, { expire_after_seconds: 3600 })
end
```

## 備考

以下のようなケースでは大量のデータ削除が行われてしまい、サーバーへの負荷が急増する可能性があるため要注意

- 既にあるドキュメントに TTL Index を設定
- 既に設定済みの TTL の有効期間を短縮

対応策として、事前に小さめのバッチ単位でドキュメントを削除しておくと良いらしい

## 参考

- [TTL Indexes - データベース マニュアル - MongoDB Docs](https://www.mongodb.com/ja-jp/docs/manual/core/index-ttl/)
- [ruby on rails - Mongoid document Time to Live - Stack Overflow](https://stackoverflow.com/questions/14685271/mongoid-document-time-to-live)
