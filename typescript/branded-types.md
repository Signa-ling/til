# Branded Types

## 前提

TypeScript は構造的型システムである。平たく言うと、「同じ構造の型は同一のものとみなされる」というもの。

```ts
type Dog = {
  name: string;
  breed: string;
}

type Cat = {
  name: string;
  breed: string;
}

const dog: Dog = { name: "ポチ", breed: "柴犬" };
const cat: Cat = dog; // 構造が同じなのでエラーにならない
```

## Branded Types の概要

Branded Types は既存の型に「ブランド」を付与することで、擬似的に名前的型システム (nominal typing) を実現する

### Nominal Typing

例えば Java などにみられるもので、同じ型構造でも型の名前や宣言から互換性を判断する

```java
class Dog {
  String name;
  String breed;
}

class Cat {
  String name;
  String breed;
}

Dog dog = new Dog();
Cat cat = dog; // 型名が Cat ≠ Dog なのでエラーになる
```

### Branded Types の実装例

```typescript
type Dog = {
  name: string;
  breed: string;
} & { __brand: 'Dog' };

type Cat = {
  name: string;
  breed: string;
} & { __brand: 'Cat' };

const dog = { name: "ポチ", breed: "柴犬" } as Dog; // as アサーションがないとこの例だとエラーになることに留意
const cat: Cat = dog; // エラーになる
```

実装方法はいくつかあるようだが、慣習としては `__brand` が一般的っぽい

## 備考

特にプリミティブ型に対して効果を発揮するもので、そこら辺はタグ付きユニオンと差別化されてるかも

## 参考

- [TypeScript の型安全性を高める Branded Types](https://zenn.dev/farstep/articles/typescript-branded-types)
- [Branded Types | Learning TypeScript](https://www.learningtypescript.com/articles/branded-types)
