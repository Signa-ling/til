# Support for exit code 62

MongoDB を 4.2 → 6.0 にアップグレードを行ったところ exit code 62 が発生し、MongoDB のコンテナが起動できなくなった。
もう少しバージョンを刻んで上げてみようと試みたところ、4.4 → 5.0 の段階で発生していた。

exit code 62 は公式によると

> --dbpath のデータファイルが現在実行中の mongod のバージョンと互換性がない場合に、mongod によって返されます。
>
> アップグレード後のデータの非互換性の問題を解決するには、MongoDB のバージョンのリリースノートを参照し、「互換性に影響する変更」を検索してください。

とのこと。

自分の場合はリリースノートをのアップグレード手順の項を参考に、以下の順でアップグレードを行った。

1. 4.4 の段階で mongo container を起動 → コンテナ内で `db.adminCommand({ setFeatureCompatibilityVersion: "4.4" })` にする
2. 5.0 に上げて mongo container を起動 → 上と同様に `db.adminCommand({ setFeatureCompatibilityVersion: "5.0" })` にする
3. 6.0 に上げて mongo container を起動 → またまた同様に `db.adminCommand({ setFeatureCompatibilityVersion: "6.0" })` にする

なおコンテナ内で `db.adminCommand` を叩く際には以下のコマンドを実行する。

- 4.4, 5.0: `docker exec -it fo-dev-mongodb-1 mongo`
- 6.0 : `docker exec -it fo-dev-mongodb-1 mongosh`
  - 6.0 から従来の mongo shell が含まれなくなったため
  - 実際は 5.0 の段階で非推奨になっている

他にもデータの参照先のパスがアップグレードによって変更されたために発生するケースもあるようだが、これについては影響していなかった。

## 参考

- [自己管理型配置の終了コードとステータス - MongoDB マニュアル v 7.3](https://www.mongodb.com/ja-jp/docs/rapid/reference/exit-codes/)
- [MongoDB 6.0 のリリースノート - MongoDB マニュアル v7.3](https://www.mongodb.com/ja-jp/docs/rapid/release-notes/6.0/#upgrade-procedures)
- [レガシーmongo shell - MongoDBマニュアル v5.0](https://www.mongodb.com/ja-jp/docs/v5.0/reference/program/mongo/)
