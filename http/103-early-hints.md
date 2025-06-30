# 103 Early Hints

平たくいうと、このステータスコードによってページ表示が高速化され、 UX の向上などにつながる

- クライアントからリクエストが送られた時に、サーバーが本レスポンス (e.g. 200 OK) などを返す前に返却するステータスコード
- サーバー側はこのコードを返却する際に、後に必要となるリソース (e.g. CSS) のリンク情報を含めて返す
  - ブラウザ側はこのレスポンスが返ってきたら、本レスポンスが返ってくる前にリソースのダウンロードを開始できる
- サーバーから本レスポンスと HTML 本文が返ってきた時にはリソースのダウンロードが済んでいるため、ページ描画がスムーズに

デメリットとしては以下の点が出てくる。

- アプリケーションサーバー側での実装の負担
  - レスポンスを2段階…つまり、103と本レスポンスに分離する必要がある
- アプリケーションサーバーだけでなくプロキシも 103 に対応している必要がある

## 余談

- このステータスコードを知ったきっかけが 2025/06/24 に nginx が v1.29.0 になったことで 103 をサポートしたことにある
  - [NGINX Introduces Support for 103 Early Hints](https://blog.nginx.org/blog/nginx-introduces-support-103-early-hints)
- 今後 Web パフォーマンス最適化の観点で考慮すべき点として、より一般的なものになってくるかもしれない

## 参考

- [103 Early Hints - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/103)
- [103 Early Hints とは何か？ ～マイナーだけど革新的なHTTPステータスの世界～](https://tech.repro.io/entry/2025/01/28/125334)
- [NGINX Introduces Support for 103 Early Hints](https://blog.nginx.org/blog/nginx-introduces-support-103-early-hints)
