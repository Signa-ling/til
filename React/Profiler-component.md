# Profiler Component

React ツリーのレンダリングパフォーマンスを測定するためのコンポーネント

## 使い方

測定したいコンポーネントを `<Profiler>` でラップしてあげればよい。また、この時に `onRender` コールバックを渡す必要がある

```tsx
const onRender = (
  id: string,
  phase: "mount" | "update" | "nested-update",
  actualDuration: number,
  baseDuration: number,
  startTime: number,
  commitTime: number
) => {
  // 計測内容についてのログを出したりする
}

<Profiler id="Sample" onRender={onRender}>
  <Sample />
</Profiler>
```

## 計測指標

詳細はリファレンスに書いてあるので割愛するが、レンダリング時の時間に関する測定しかできない（メモリの使用量などは測れない）

## 参考

- [<Profiler> – React](https://ja.react.dev/reference/react/Profiler)
