# infer type predicates

v5.5 から使えるもので従来は

```ts
function isNumber(value: number | string): value is number {
  return typeof value === 'number';
}
```

と書いていたものを

```ts
function isNumber(value: number | string) {
  return typeof value === 'number';
}
```

のように書ける。

従来手法にあるように、ユーザー定義型ガード (e.g. `value is string`) を使えば型は絞り込むことができるけど、ユーザーが定義している部分なのでコンパイルエラーにならず、安全性に欠ける。

infer type predicates のおかげでより型安全に記述ができるのでハッピー！

## 参考

- [TypeScript 5.5で型述語を推論できて最高。配列のfilterも型安全に](https://zenn.dev/ubie_dev/articles/ts-infer-type-predicates)
- [Type Predicate Inference: The TS 5.5 Feature No One Expected | Total TypeScript](https://www.totaltypescript.com/type-predicate-inference)
