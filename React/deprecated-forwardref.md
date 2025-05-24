# Deprecated forwardref

## React 18 以前

`forwardRef` を使ってコンポーネント関数 (`render`) をラップする必要があった。

```tsx
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  // 中略
});
```

## React 19 から

`ref` を `props` で渡せるようになった。

```tsx
function MyInput({props, ref}) {
  // 中略
}

<MyInput ref=ref />
```

これによって将来的に forwardref が非推奨になり削除されるとのこと。

## forwardref はなぜ必要だったか？

そもそも `ref` は

- `ref` は [Special Props](https://react.dev/warnings/special-props) として定義されているため、`props` として渡せない
- `ref` オブジェクト自体は別の名前にすることで渡すことができる

という性質になっている。

それを踏まえた `forwardref` の利点については[このような議論](https://gist.github.com/gaearon/1a018a023347fe1c2476073330cc5509)がされている。

ここで言われてることとして、`ref` prop によって `ref` を渡せることで、 API の一貫性や予測可能性が高まるが大きいとのこと。 （特にサードパーティライブラリ）

## 参考

- [React v19 – React](https://ja.react.dev/blog/2024/12/05/react-19#ref-as-a-prop)
- [forwardRef – React](https://ja.react.dev/reference/react/forwardRef)
- [dom_ref_forwarding_alternatives_before_16.3.md](https://gist.github.com/gaearon/1a018a023347fe1c2476073330cc5509)
