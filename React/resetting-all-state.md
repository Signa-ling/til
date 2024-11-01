# Resetting all state

コンポーネントの state を全て初期化する時の方法として、コンポーネント自体を再生成する手段がある

再生成するのは簡単で、コンポーネントの `key` を変更するだけ

雑なイメージ

```tsx
const HogeComponent = (): JSX.Element => {

}

const HogePage = (): JSX.Element => {
  const [key, setKey] = useState(0); // ランダムな値だと尚良いかも

  // これがどこかで呼ばれたら key が変わる
  const resetComponent = () => {
    setKey(key + 1);
  }

  // key が変わるとこのコンポーネント自体が作り直される
  return
  <>
    <ResetButton onClick={resetComponent} />
    <HogeComponent key={key}/>
  </>
}
```

React の動作として
- 同じコンポーネントが同じ場所でレンダーされるときに state を保持する
- key が変更されるたびに、DOM が再作成され、Key を渡しているコンポーネントとそのすべての子コンポーネントの state をリセットする

## 参考

- [props が変更されたときにすべての state をリセットする](https://ja.react.dev/learn/you-might-not-need-an-effect#resetting-all-state-when-a-prop-changes)
