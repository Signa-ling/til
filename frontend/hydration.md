# Hydration

SSR(Server Side Rendering) や SSG (Static Site Generator)などから事前生成された HTML に対してイベントハンドラをアタッチしていく処理のこと。これによって静的ページを動的ページへ変換する。

ちなみに Rehydration とも表現されている（同義なのかは分からない）

## 簡単なイメージ

以下のような事前生成された HTML があったとき、この中の `<button>` に対してクリックイベントをアタッチする。

```html
<body>
  <div id="root">
    <button><!-- ここに onClick を後から付与するイメージ--->
      add
    </button>
  </div>
</body>
```

## 利点と欠点

利点として、先行して静的ページを表示させることで、ユーザーの待ち時間を減らすことができる。

反面、イベントハンドラがアタッチされるまではユーザーは入力などを行えない。そのため、ユーザーへの混乱やストレスを与える。

つまり、FCP が改善される反面、INP や TBT への悪影響を及ぼす可能性がある。

## 備考

適切な使い方をすることでパフォーマンス向上に繋がるとのことで、以下のようなバリエーションがあるらしい（学んだら追記予定）

- Streaming Server-Side Rendering
- Progressive rehydration
- Partial rehydration
- Trisomorphic rendering

## 参考

- [Rendering on the Web  |  Articles  |  web.dev](https://web.dev/articles/rendering-on-the-web#rehydration)
