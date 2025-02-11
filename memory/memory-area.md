# Memory Area

大きく以下の4つに分かれる

- テキスト領域: 機械語に翻訳されたプログラムがロードされる領域
- 静的領域: グローバル変数などの静的が置かれる領域
- スタック領域: 関数のローカル変数に束縛された値（コンパイル時にサイズが決まるもの）が置かれる領域
- ヒープ領域: プログラムの実行中に動的に確保されてデータが置かれる領域

もう少し詳しく見ようとするなら[この画像](https://pbs.twimg.com/media/DXBhvZ4WsAAAJdS?format=jpg&name=large)が分かりやすそう

## 参考

- [メモリの 4 領域](https://brain.cc.kogakuin.ac.jp/~kanamaru/lecture/MP/final/part06/node8.html)
- [コンセプトから理解する Rust](https://gihyo.jp/book/2022/978-4-297-12562-2)
  - P.160