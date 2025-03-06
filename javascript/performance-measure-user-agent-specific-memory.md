# Performance#measureUserAgentSpecificMemory()

ウェブアプリケーションのメモリ使用量推定に使われる。

2025/02/27 時点ではこの機能は実験的なものであり、ブラウザによっては互換性が無い

また、使用する際にはクロスオリジン分離として以下をヘッダーに設定する必要がある

```http
Cross-Origin-Opener-Policy: same-origin
Cross-Origin-Embedder-Policy: require-corp # credentialless でも可
```

## 使い方

```javascript
if (performance.measureUserAgentSpecificMemory) {
  performance.measureUserAgentSpecificMemory()
    .then((result) => {
      console.log(result);
    })
    .catch((err) => {
      console.error(err);
    });
} else {
  console.warn('measureUserAgentSpecificMemory はサポートされていません');
}
```

上記のように書くと、以下のような情報を取得できる

```javascript
{
  bytes: 12345678, // 使用されているメモリの合計（バイト単位）
  breakdown: [ // 各コンテキスト（window や worker）ごとの内訳
    {
      bytes: 123456,
      attribution: ["Window", "https://example.com"]
    },
    {
      bytes: 98765,
      attribution: ["DedicatedWorker", "https://example.com/worker.js"]
    },
    // ...
  ]
}
```

## 参考

- [Performance: measureUserAgentSpecificMemory() メソッド - Web API | MDN](https://developer.mozilla.org/ja/docs/Web/API/Performance/measureUserAgentSpecificMemory)
