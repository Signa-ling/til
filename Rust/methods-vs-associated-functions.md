# Methods vs Associated Functions

## 概要

Rust にはメソッドと関連関数がある

実装例を交えると以下の通り

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // メソッド: &self を持つ
    fn area(&self) -> u32 {
        self.width * self.height
    }

    // 関連関数: &self を持たない
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let rect = Rectangle { width: 10, height: 20 };

    // メソッドの呼び出し: インスタンスに対してドット記法で呼び出す
    println!("Area: {}", rect.area());

    // 関連関数の呼び出し: 型に対して :: を使って呼び出す
    let sq = Rectangle::square(5);
    println!("Square: {}x{}", sq.width, sq.height);
}
```

つまり、自分自身を引数に取るのがメソッド、自分自身を引数に取らないのが関連関数である。
クラスの概念と比較すると以下の通り

- インスタンスメソッド: Rust のメソッドに相当
- クラスメソッド: Rust の関連関数に相当

## 参考

- [メソッド記法 - The Rust Programming Language 日本語版](https://doc.rust-jp.rs/book-ja/ch05-03-method-syntax.html)
