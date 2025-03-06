# Tree Shaking cannot remove class methods

Javascript をバンドルする上でデッドコードを除去してファイル出力の最適化をすることを Tree Shaking というらしい。これにより、不要なコードのバンドルを防ぐことができる。

Tree Shaking をするにはいくつかの条件がある。そのうちの1つに ECMAScript Module(ESM) の `import`, `export` の利用が必要になる。

これは Tree Shaking が ESM の静的な構造を解析して、コードの使用状況を判断するためである。

ここでクラスメソッドは一般に削除されない。

```ts
export class Sample {
  public static Foo() { // これが未使用でも一般には削除されない
    console.log("foo");
  }
 }
```

理由は以下の2つ

- クラスは単一の構造体として `export` されているが、メソッド単位では使用状況が分からない
- `this` を使った動的な呼び出しをしている可能性があり、静的解析では確実に未使用かを判断できない

ただし、クラス自体が一度も使用されていなければクラスごと削除される。そのため、結果的にクラスメソッドも削除できる。

## 参考

- [Tree Shaking | webpack](https://webpack.js.org/guides/tree-shaking/)
- 以下 rollup, webpack での class method に対する Tree shaking に関する issue （2025/03/06 時点ではまだ未対応っぽい？）
  - [Tree shaking class methods · Issue #349 · rollup/rollup](https://github.com/rollup/rollup/issues/349)
  - [Tree shaking class methods · Issue #13922 · webpack/webpack](https://github.com/webpack/webpack/issues/13922)
