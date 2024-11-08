# useEffect is called twice

React v18 以降は `useEffect` が2回呼ばれるようになった。

これは v18 以降の仕様で、厳密には StrictMode が有効な場合、開発時にこのような挙動をする。

これへの正しい対応はエフェクトを1回だけ実行するのではなく、再マウントされても正しく動作するようクリーンアップ関数を実装すること。

クリーンアップ関数はエフェクトが行っていたことを停止、または元に戻すといった処理を行うものを指す。

```tsx
"use client";

import { useEffect } from 'react';

export default function MyComponent() {

  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setCount(count + 1);
    }, 1000);

    return () => clearInterval(interval);
  }, [count])

  return (
    <div>
      <h1>Count: {count}</h1>
    </div>
  );
}
```

非推奨の対応としては StrictMode を無効化する、 `useRef`を使うなどが挙げられる。

なおこのような挙動をするようになった理由は以下の通り。

> コンポーネントを再マウントすることで、React はページを離れて戻ってきてもコードが壊れないことを確認します。

## 参考

- [エフェクトを使って同期を行う – React](https://ja.react.dev/learn/synchronizing-with-effects#how-to-handle-the-effect-firing-twice-in-development)
