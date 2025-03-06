# Add HTTP Header

`next.config.js` に以下のように記述すると付与できる

```js
module.exports = {
  async headers() {
    return [
      {
        source: '/about', // ヘッダーを付与したいパスパターン。正規表現を使うこともできる
        headers: [
          {
            // 付与したいヘッダー毎に key: value 形式で定義
            key: 'x-custom-header',
            value: 'my custom header value',
          },
          {
            key: 'x-another-custom-header',
            value: 'my other custom header value',
          },
        ],
      },
    ]
  },
}
```

## 余談

これやろうとした経緯が、

1. [performance.measureUserAgentSpecificMemory()](https://developer.mozilla.org/en-US/docs/Web/API/Performance/measureUserAgentSpecificMemory#examples) を使ってメモリ使用量を調べたい
2. これを実現するには[クロスオリジン分離](https://developer.mozilla.org/en-US/docs/Web/API/Window/crossOriginIsolated)をしないといけない（つまり、ヘッダーにいくつか追加が必要）

という流れだった。

## 参考

- [ヘッダー | Next.js 日本語ドキュメント](https://nextjsjp.org/docs/app/api-reference/config/next-config-js/headers)
