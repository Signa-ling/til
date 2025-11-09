# Clap basic usage

## 概要

コマンドライン引数をパースして Rust の型に変換するクレート、つまりコマンドライン引数パーサーである

例えば以下のようなコマンドを入力する

```sh
$ sample-todo add "買い物リスト" --tags 生活,買い物
```

これを Rust 側では解析して以下のような構造体に変換することで Rust 上で扱えるようにする

```rust
Commands::Add {
  title: "買い物リスト",
  tags: vec!["生活", "買い物"],
}
```

### 定義方法

```rust
use clap::{Parser, Subcommand};

#[derive(Parser)] // parse するのに必要な関数などを提供
#[command(name = "sample-todo")] // help コマンドなどを実行した時に表示されるこのアプリの名前
#[command(about = "サンプルアプリ")] // help コマンドなどを実行した時に表示される説明
pub struct Cli {
    #[command(subcommand)] // subcommand の定義
    pub command: Commands,
}

#[derive(Subcommand)] // subcommand の利用に必要な関数などを提供
pub enum Commands {
  Add {
    title: String, // 位置引数として扱われる。つまり $ sample-todo add "hogehoge" の "hogehoge" の部分

    // short, long: フィールド名から自動的に -t,  --tags のようなオプション名を生成
    // value_delimiter: 指定された文字列区切り（以下ならカンマ）で複数の値を受け取る
    #[arg(short, long, value_delimiter = ',')]
    tags: Vec<String>,
  }
}
```

### 呼び出し方

```rust
fn main() {
    let cli = Cli::parse();  // コマンドライン引数をパース

    match cli.command {
        Commands::Add { title, tags } => {
            println!("Title: {}", title);
            println!("Tags: {:?}", tags);
        }
    }
}
```

## 備考

`#[command(name = "...")]` の部分は `$ command --help` などをした時に表示される説明上の名前で、アプリ名と一致してなくても動く。

つまり、 Cargo.toml の方でアプリ名を hoge にしておき、 `#[command(name = "fuga")]` のように書くようなことができる。

が、これをすると

```sh
$ hoge --help # これは動く

# 以下に hoge アプリの説明が表示されるが fuga というアプリ名で説明される
Usage fuga

# 中略

# 一方で以下は fuga というアプリ名で build されるわけではないため、動かない
$ fuga --help
```

といった状態になる。基本的には Cargo.toml に記載したアプリ名と揃えるのが良い。

## 参考

- [clap - Rust](https://docs.rs/clap/latest/clap/)
