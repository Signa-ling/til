# Unused Warning Reachability Based

## 概要

Rust のコンパイラは未使用な宣言がある場合にその旨を警告する

```rust
fn unused_func() -> i32 {
    1
}

// warning: function `unused_func` is never used
//  --> src/lib.rs:1:4
//   |
// 1 | fn unused_func() -> i32 {
//   |    ^^^^^^^^^^^
//   |
//   = note: `#[warn(dead_code)]` on by default
```

ここで「自分は親関数から呼ばれているが、親関数が未使用である場合」を考える

```rust
fn self_func() -> i32 {
    1
}

fn parent_func() -> i32 {
  1 + self_func()
}
```

その場合の警告は下記のようになる

```sh
warning: function `self_func` is never used
 --> src/lib.rs:1:4
  |
1 | fn self_func() -> i32 {
  |    ^^^^^^^^^
  |
  = note: `#[warn(dead_code)]` on by default

warning: function `parent_func` is never used
 --> src/lib.rs:5:4
  |
5 | fn parent_func() -> i32 {
  |    ^^^^^^^^^^^
```

つまり、エントリポイントから見て呼ばれないもの全てが対象になっている（=到達可能性により判断している）

（`parent_func` は呼ばれない = 到達しないから警告対象、 `self_func` も `parent_func` からなら呼ばれるが、 `parent_func` が呼べないため到達できず、警告対象になる）

## 備考

`#[allow(dead_code)]` を付与することで警告を無視できる

```rust
#[allow(dead_code)]
fn unused_func() -> i32 {
    1
}
```

## 参考

- [dead_code - Rust By Example](https://doc.rust-lang.org/rust-by-example/attribute/unused.html)
- [Replace visibility test with reachability test in dead code detection](https://github.com/rust-lang/rust/pull/119552)
