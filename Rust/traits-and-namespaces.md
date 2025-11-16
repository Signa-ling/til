# Traits and Namespaces

## 概要

以下のような型とトレイト実装があるとする

```rust
struct Fuga;

trait Hoge {
    fn hoge_func(&self);
}

impl Hoge for Fuga {
    fn hoge_func(&self) {
        println!("Hoge trait method");
    }
}
```

この時、 Hoge を trait（トレイト）、hoge_func を trait method（トレイトメソッド） という。
`impl Hoge for Fuga` のように書くことで「Hoge trait の内容を借りて Fuga Type で扱えるようにする」ということになる。

借りたトレイトメソッドを利用する際には、トレイトをスコープに入れる必要がある

```rust
use Hoge; // トレイトの宣言

let fuga = Fuga;
fuga.hoge_func(); // インスタンスメソッドとして呼び出し
```

なぜトレイトの宣言が必要になるかというと、 `hoge_func()` は Hoge トレイトの名前空間に属しているため。
`Fuga` 型が実装しても、メソッドは Hoge の借り物のままなので、Hoge トレイトをスコープに入れる必要がある。

### 名前空間の衝突回避

以下のように異なるトレイトに同じ名前のインスタンスメソッドが存在したとする

```rust
struct Fuga;

trait TraitA {
    fn some_func(&self);
}

trait TraitB {
    fn some_func(&self);
}

impl TraitA for Fuga {
    fn some_func(&self) { println!("TraitA"); }
}

impl TraitB for Fuga {
    fn some_func(&self) { println!("TraitB"); }
}
```

この時、

```rust
let fuga = Fuga;
fuga.some_func();
```

とすると、 `TraitA` によるものか `TraitB` によるものかが判断できずエラーになる。

そのため、以下のような方法で呼び出しているトレイトメソッドがどこのトレイトのものかを明示する必要がある

```rust
// 方法1：トレイトをインポート
use TraitA;
fuga.some_func(); // TraitAのsome_funcを呼び出す

// 方法2：完全修飾構文（useを省略）
TraitA::some_func(&fuga);           // パターン1
<Fuga as TraitA>::some_func(&fuga); // パターン2（最も明確）

// TraitBを使いたい場合
TraitB::some_func(&fuga);
<Fuga as TraitB>::some_func(&fuga);
```
