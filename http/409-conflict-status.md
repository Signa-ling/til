# 409 Conflict Status Code

リクエストが現在のサーバーの状態と競合したことを示すコード

## 発生し得るケース

- サーバー上の既存ファイルより古いファイルをアップロードした時（バージョン制御による競合）
- サーバーが同じリソースを更新する複数リクエストを受け取った場合


## 参考

- [409 Conflict - HTTP | MDN](https://developer.mozilla.org/ja/docs/Web/HTTP/Reference/Status/409)
