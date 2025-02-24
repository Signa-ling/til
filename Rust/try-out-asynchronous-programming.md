# Try out asynchronous programming

Rust には非同期プログラミングの仕組みとして `async` / `await` がある。

しかし、標準状態ではランタイムが用意されておらず扱うのが難しいので、クレートに頼るのが吉。

## 前提

例えば、以下のような `async` を付与した関数とそれを呼び出す部分がある。

```rust
async fn sum_func(n: usize) -> usize {
    let ans = (1..=n).sum::<usize>();
    println!("{}", ans);

    ans
}

fn main() {
    sum_func(10000000);
}
```

Javascript などと違い、Rust では `async` を付与した関数は呼び出し時に `impl Future<Output=T>`型の値を返すだけで、関数の処理をしない。したがって、上記の状態では計算処理が行われない。

この状態でコンパイルすると「`.await` の実行または、`poll()` を呼ばなければ実行されない」という記載が返ってくる。そのため、`.await` か `poll()` を呼ぶことで解決する事がわかる。

## await と poll

### `poll()`

- `Future` トレイトが定義しているメソッド
- 非同期処理の進捗状況を問い合わせる
  - これを1回呼ぶだけでは、完了していない場合が多い
  - なのでループ内で何度も呼び出す必要がある

### `await`

- `poll` のループを自動でやってくれるシンタックスシュガー
- 一般にはランタイムが `poll` を管理してくれているので、利用側は `await` を付与するだけで使える

## `poll()`

そもそもランタイムが標準で提供されていないため、`poll()` するには自分で最低限のランタイムを用意する必要がある。

が、これは安全性や簡便性などの面からも非推奨。よって、学習目的でない限りはやらなくてよいと思う。

## `.await` を付与する

標準で `poll()` が無いのでランタイムを用意していなければ使えない。

ちなみに、`poll` が無い状態で以下のように記述すると、「`await` は `async` 関数または `async` ブロック内でのみ許可されている」というエラーが出てくる。

```rust
fn main() {
    sum_func(10000000).await;
}
```

次に `main()` に `async` を付与する。

```rust
async fn main() {
    sum_func(10000000).await;
}
```

すると、今度は「`main()` に `async` を使えない」というエラーになる。

`async` / `await` は `async` を付与した関数を呼び出してる関数に `async` を付与、それを呼び出してる関数にも `async` を…と親関数へ付与が伝播する。
よって、標準のまま書こうとすると `main` へ書く必要が出てくる…はず。なので、このままでは書けない。

使えない理由の一次ソースは見つけられなかったが、 [main() が非同期だと誰が main() をポーリングするのか？](https://stackoverflow.com/a/74012183) という話も出ていて、確かにとなった。

## クレートに頼る

これが1番良い。

定番のクレートとしては [Tokio](https://crates.io/crates/tokio) がある。

### インストール

`Cargo.toml` に以下を記述することで、使えるようになる。

```toml
[dependencies]
tokio = { version = "x.yy.z", features = ["full"] }
```

version はこれを書いてるときは 1.43.0 であった。

features は Tokio の利用したい機能に応じて適宜記述を変えると良い。 Tokio には色々な機能が備わっているが、すべてのアプリケーションにすべての機能が必要というわけではない。コンパイル時間などの最適化を考慮すると、必要に応じた選択をしたほうが良い。

### コード例

attribute を使わない場合、以下のように書くと `println!` の内容が出力される。

```rust
use tokio::{runtime::Builder, task::LocalSet};

async fn sum_func(n: usize) -> usize {
    let ans = (1..=n).sum::<usize>();
    println!("{}", ans);
    ans
}

fn main() {
    let future = sum_func(10000000);

    let local_set = LocalSet::new();
    let runtime = Builder::new_multi_thread().enable_all().build().unwrap();
    local_set.block_on(&runtime, future);
}
```

が、長いので attribute を使った書き方が一般的。以下は main 関数に attribute を付与している。

```rust
#[tokio::main]
async fn main() {
    sum_func(10000000).await;
}
```

これでも同様の結果が得られる。

## 参考

- [コンセプトから理解する Rust](https://gihyo.jp/book/2022/978-4-297-12562-2)
  - P.321 ~ 329
- [How to use async/await in Rust when you can't make main function async - Stack Overflow](https://stackoverflow.com/questions/71116502/how-to-use-async-await-in-rust-when-you-cant-make-main-function-async)
- [Hello Tokio | Tokio - An asynchronous Rust runtime](https://tokio.rs/tokio/tutorial/hello-tokio)
