# Package Crate Module

## 概要

- Package: 配布や管理をする単位
  - Ruby でいう Gem
  - ライブラリと言われることもあるが、公式的にはそのような概念では説明されない
- Crate: コンパイル（人間の書いたコードを機械語に翻訳する）単位
  - バイナリクレートとライブラリクレートの2種類がある
    - バイナリクレート: 実行可能なファイルに変換される対象、 `main.rs` を起点にする
    - ライブラリクレート: 他の Rust PJ でも再利用可能なファイルに変換される対象、 `lib.rs` を起点にする
- Module: コード整理の単位
  - 名前空間と公開範囲から成り立つ仕組み
  - `mod hogehoge` みたいな書き方をする

## 参考

- [パッケージとクレート - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/ch07-01-packages-and-crates.html)
