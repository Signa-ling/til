# Environment Variables

以下のような環境変数があるとして、

```.env
SAMPLE="sample"
```

Next.js では以下のような形式でその環境変数を取得できる

```ts
const sampleVariable = process.env.SAMPLE;
console.log(sampleVariable); // => sample
```

サーバー側だとこれで問題ないが、クライアント側で環境変数を使う場合は prefix として `NEXT_PUBLIC_` の付与が必要

```.env
NEXT_PUBLIC_SAMPLE="sample"
```

```ts
// クライアント側
const sampleVariable = process.env.NEXT_PUBLIC_SAMPLE;
console.log(sampleVariable); // => sample
```

prefix が無い場合、クライアント側で呼び出しても undefined が返ってくる

```ts
// クライアント側
const sampleVariable = process.env.SAMPLE;
console.log(sampleVariable); // => undefined
```

## セキュリティ面の備考

`NEXT_PUBLIC` を付与することでクライアント側で扱えるようになると、今度はこの値がブラウザから閲覧できるということになる。

そのため、公開するとまずい情報 (e.g. API KEY) を含んでしまっていないか注意する必要がある。

## 参考

- [Configuring: Environment Variables | Next.js](https://nextjs.org/docs/pages/building-your-application/configuring/environment-variables)
