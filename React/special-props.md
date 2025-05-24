# Special Props

`props` には所謂予約語が存在しており、これらの名前をコンポーネントに渡すには別のプロパティとして定義する必要がある。

それが、 `ref` と `key` である。

[この部分](https://github.com/facebook/react/blob/f14d7f0d2597ea25da12bcf97772e8803f2a394c/packages/react/src/jsx/ReactJSXElement.js#L417-L447)で `props` として `key`, `ref` を使っていた場合は除外処理を行っているらしい。

```tsx
// key, ref は Sample に渡されない
<Sample key={key} ref={ref} value={value} />

// 裏を返すと、 Sample 側での props の定義に key, ref は記載しなくて良い
const Sample = ({ value }) => {
  // 中略
}

// あくまでも予約語なので、 key, ref に渡す値を使いたい場合別名で定義すればいい
<Sample2 key={key} dataKey={key} />

const Sample2 = ({ dataKey }) => {
  // 中略
}
```

## 調べたきっかけ

`forwardref` の必要性について調べてた時に引っかかった [zenn の記事](https://zenn.dev/tocomi/scraps/a04f8b5df18cb4)から知った。

## 参考

- [Special Props Warning – React](https://react.dev/warnings/special-props)
- [react/packages/react/src/jsx/ReactJSXElement.js at f14d7f0d2597ea25da12bcf97772e8803f2a394c · facebook/react](https://github.com/facebook/react/blob/f14d7f0d2597ea25da12bcf97772e8803f2a394c/packages/react/src/jsx/ReactJSXElement.js#L417-L447)
