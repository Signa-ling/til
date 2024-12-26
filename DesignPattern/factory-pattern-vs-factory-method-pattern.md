# Factory Pattern vs Factory Method Pattern

輪読会の中で出た話だったので

共通事項としては「オブジェクト生成をクライアントに意識させない」ことが目的らしい

以下の動物クラスを例にそれぞれの実装パターンを見る

```ruby
# 親クラス
class Animal
  def speak
    raise NotImplementedError, "You must implement speak in a subclass"
  end
end

# 子クラス
class Dog < Animal
  def speak
    "Woof!"
  end
end

class Cat < Animal
  def speak
    "Meow!"
  end
end
```

## Factory Pattern

- 単一の Factory Class を用意し、その中のメソッドでクラスを生成をする
  - この時 `if-else` や `case` などの分岐によって生成するクラスを切り替えることが多い

```ruby
# Factory クラス
class AnimalFactory
  def self.create(type)
    case type
    when :dog
      Dog.new
    when :cat
      Cat.new
    else
      raise "Unknown animal type: #{type}"
    end
  end
end

# 使用例
dog = AnimalFactory.create(:dog)
puts dog.speak
# => "Woof!"

cat = AnimalFactory.create(:cat)
puts cat.speak
# => "Meow!"
```

### どういう時に嬉しいか

- 生成ロジックがそこまで複雑ではないが、複数種類のオブジェクトを作り分けたい
- クライアントコードをシンプルにしたい
- 複数の場所で同じように `new` している処理の一元化

## Factory Method Pattern

- 生成をサブクラスに任せる
  - これにより、生成すべきオブジェクトを決めるメソッド (= Factory Method) を抽象化できる
- 抽象クラスに Factory Method を定義し、サブクラスで生成部分をオーバーライドする

```ruby
# 抽象クラス（Factory クラス）: AnimalShelter
# - adoptメソッドは共通で使いたい処理
# - create_animal はサブクラスに作成を任せる (Factory Method)
class AnimalShelter
  def adopt
    animal = create_animal
    # adopt の流れの中で speakさせるイメージ
    puts "You adopted a new pet. It says: #{animal.speak}"
    animal
  end

  def create_animal
    # 具体的な動物生成はサブクラスに委ねる
    raise NotImplementedError, "You must implement create_animal in a subclass"
  end
end

# サブクラス
class DogShelter < AnimalShelter
  def create_animal
    Dog.new
  end
end

class CatShelter < AnimalShelter
  def create_animal
    Cat.new
  end
end

# 使用例
dog_shelter = DogShelter.new
new_dog = dog_shelter.adopt
# => You adopted a new pet. It says: Woof!

cat_shelter = CatShelter.new
new_cat = cat_shelter.adopt
# => You adopted a new pet. It says: Meow!
```

### どういう時に嬉しいか

- 共通の処理フローがあるが、作るものを差し替えたい
  - 共通化したくない処理だけサブクラスに投げ、親クラスのテンプレートメソッドで処理の流れ全体を定義できる
    - 共通化できる部分と差し替え（サブクラスごとに分けたい処理）部分を明示的にできる
  - `new` するときだけじゃ無く、生成したオブジェクトを使った追加処理もまとめられる
- サブクラスごとの拡張が増えそう

## どちらがいいか

結論としてはケースバイケースだが、それぞれ以下のようなデメリットを抱えている

- Factory Pattern: Factory クラス内で作成するレパートリーが増えると分岐が肥大化する
- Factory Method Pattern: 抽象クラスの概念が加わって構造が複雑化する

つまり、

- Factory Pattern: シンプルだが、変更に弱い（Factory が肥大化する）
- Factory Method Pattern: 抽象的だが、拡張に強い設計となりやすい

と言える。

そのため、単純なケースなら Factory Pattern を、複雑なケースなら Factory Method Pattern を使うと良いと思う。

## 感想

Factory Pattern はシンプルだから分かりやすいが、 Factory Method Pattern はちょっとややこしい…
特に後者は、実際にアプリケーションコードで使ってみないと覚えられない気がする…。
いい感じの使用例が知りたいかも。

## TODO

関連内容として、 Abstract Factory パターンもあるらしいので追々調べてみる

## 参考

コードの具体例など含め ChatGPT に丸投げしたのでリファレンスはないが、ここを読んでおくといいかもしれない

- [ファクトリーの比較](https://refactoring.guru/ja/design-patterns/factory-comparison)
- [Factory Method](https://refactoring.guru/ja/design-patterns/factory-method)
