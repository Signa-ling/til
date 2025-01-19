# Represent Class

Rust に Class の概念は無いが、 Class と同じようなものを表現する方法がある。

ここでは自分の書き慣れている Ruby と比較してどう実装するかを書いておく。

色々な考慮パターンはあると思うが、とりあえずシンプルな単一クラスと、継承が発生する場合だけを抑えておく。

## 単一クラスを考える

Rectangle class があったとする

```ruby
class Rectangle
  def initialize(width, height)
    @width = width
    @height = height
  end

  def calc_area
    @width * @height
  end
end

rectangle = Rectangle.new(10, 30)
puts(rectangle.calc_area) # => 300
```

これを Rust で実装する場合、構造体にデータを定義し、そのデータ型で実装したい処理を `impl` で定義する。 

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn calc_area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rectangle = Rectangle {
        width: 10,
        height: 30,
    };

    println!("{}", rectangle.calc_area()); // 300
}
```

## 継承を考慮する

基底クラス `Shape` とそれを継承する `Triangle`, `Rectangle` について考える。

```ruby
class Shape
  # 継承先に実装がない場合はエラーを返す
  def calc_area
    raise NoImplementedError, "You must implement this method in a subclass"
  end
end

class Triangle < Shape
  def initialize(base, height)
    @base = base
    @height = height
  end

  def calc_area
    @base * @height / 2
  end
end

class Rectangle < Shape
  def initialize(width, height)
    @width = width
    @height = height
  end

  def calc_area
    @width * @height
  end
end

triangle = Triangle.new(10, 30)
puts(triangle.calc_area) # 150

rectangle = Rectangle.new(10, 30)
puts(rectangle.calc_area) # 300
```

Ruby は動的型付け言語なので、ダックタイピングが行える。しかし、インターフェースによるメソッドの強制実装はできないので、継承先での実装漏れが発生し得る。

実装が必要であることを伝える術として、基底クラスに実装したいメソッドを置いておく。このメソッドが `NoImplementedError` を返すようにすることで、この意図を伝えられる。

```ruby
class Circle < Shape
  def initialize(radius)
    @radius = radius
  end

  # calc_area を実装し忘れたとする
end

circle = Circle.new(10)
puts(circle.calc_area) # NoImplementedError が返ってくる
```

Rust は静的型付け言語なので、ダックタイピングはできない。その上、継承の概念も無いしインターフェースも存在しない。

代わりにインターフェースに相当するものとしてトレイトがある。

```rust
// この trait を満たす型には calc_area() が実装されている必要がある
trait CalcArea {
    fn calc_area(&self) -> u32;
}

struct Triangle {
    base: u32,
    height: u32,
}

// Triangle 型は CalcArea トレイトを満たすように実装する必要がある
impl CalcArea for Triangle {
    fn calc_area(&self) -> u32 {
        self.base * self.height / 2
    }
}

struct Rectangle {
    width: u32,
    height: u32,
}

impl CalcArea for Rectangle {
    fn calc_area(&self) -> u32 {
        self.width * self.height
    }
}


fn main() {
    let triangle = Triangle {
        base: 10,
        height: 30,
    };

    println!("{}", triangle.calc_area()); // 150
    
    let rectangle = Rectangle {
        width: 10,
        height: 30,
    };

    println!("{}", rectangle.calc_area()); // 300
}
```

そして、以下はコンパイルエラーになる。

```rust
struct Circle {
    radius: u32,
}

// not all trait items implemented, missing: `calc_area`
impl CalcArea for Circle {
}

fn main() {
    let circle = Circle {
        radius: 10,
    };

    // 実装が無いので駄目ではあるが、そもそも Circle が CalcArea を満たしていないのが問題なので、ここはエラーとならない
    println!("Area of Circle: {}", circle.calc_area()); 
}

```

## 感想

以前よりオブジェクト指向の理解度が上がってたおかげで、紐づけやすかった。

でも今のところは、クラスでデータと処理がまとまっていたほうが嬉しさを感じる…。

## 参考

- [コンセプトから理解する Rust](https://gihyo.jp/book/2022/978-4-297-12562-2)
  - P.114 からの構造体に関する節と、P.200 からのトレイトの制約に関する節を基に記載