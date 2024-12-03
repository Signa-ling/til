# NoInfer

型推論を防ぐための型

以下のように使うことで `getIndexFromArray` の型 T が `Fruit | 'peach'` と推論されず、 `Fruit` と推論されるようになる

```ts
function getIndexFromArray<T extends string>(
  elements: T[],
  item: NoInfer<T>
): number {
  return elements.findIndex((element) => element === item);
}
 
type Fruit = "grape" | "apple" | "banana";
const fruits: Fruit[] = ["grape", "apple", "banana"];
getIndexFromArray(fruits, "apple");
getIndexFromArray(fruits, "peach");
// ^ Argument of type '"peach"' is not assignable to parameter of type 'Fruit'.
```

ドキュメントなどを読むとなんとなく言いたいことは分かるが、実際にどういう時に使ったら良いかが分からない…。

また、[NoInfer<T>の活用例見つけた](https://qiita.com/uhyo/items/066a9f0d20cd112cf05a) という記事も読んだものの、そこまで実感できず…。

ただ、 Map in Map な状態で型推論を上手く機能させたい時に使える…という学びにはなったかもしれない

## 参考

- [NoInfer<T> | TypeScript入門『サバイバルTypeScript』](https://typescriptbook.jp/reference/type-reuse/utility-types/no-infer)
- [NoInfer<T>の活用例見つけた](https://qiita.com/uhyo/items/066a9f0d20cd112cf05a)
