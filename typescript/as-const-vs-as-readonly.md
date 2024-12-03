# as const VS as readonly

## as const

オブジェクトや配列に対する変更を防ぐ術として定番の手法だが、欠点としては `includes` が使えないことが挙げられる。

```ts
const fruits = ["grape", "apple", "banana"] as const;

fruits.push("orange");
// Property 'push' does not exist on type 'readonly ["grape", "apple", "banana"]'.
// これは防げるので嬉しい

fruits.includes("orange");
// Argument of type '"orange"' is not assignable to parameter of type '"grape" | "apple" | "banana"'.
// これが発生すると、困る
```

なのでよく `some` で対応していたが、これもベタ書きだと怒られるので関数でラッピングしないといけない。

```ts
const fruits = ["grape", "apple", "banana"] as const;

// 以下は通る
const someFruit = (fruit: string): boolean => fruits.some((f) => f === fruit);
someFruit("orange");

// これはダメ
fruits.some((f) => f === "orange");
// This comparison appears to be unintentional because the types '"grape" | "apple" | "banana"' and '"orange"' have no overlap.
```

## as readonly

`as readonly` を付与することで変更を防ぎつつ `includes` が扱えるらしい。

```ts
const fruits = ["grape", "apple", "banana"] as readonly string[];

fruits.includes("orange"); // false

fruits.push("orange");
// Property 'push' does not exist on type 'readonly string[]'.
```

ちなみに `Readonly` 型を使うでも良い

```ts
const fruits: Readonly<string[]> = ["grape", "apple", "banana"];

fruits.includes("orange"); // false

fruits.push("orange");
// Property 'push' does not exist on type 'readonly string[]'.
```

## 参考

- [Readonly<T> | TypeScript入門『サバイバルTypeScript』](https://typescriptbook.jp/reference/type-reuse/utility-types/readonly)
