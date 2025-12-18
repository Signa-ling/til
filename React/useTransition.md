# useTransition

## 概要

React18 で追加されたフックの1つ
以下のように、`useTransition` の返す `startTransition` 関数でラップされた処理（慣習でアクションという）は、バックグラウンドで実行される

```tsx
const SampleComponent = () => {
  const [isPending, startTransition] = useTransition();
  const [inputValue, setInputValueAction] = useState("");

  const handleChange = (value: string) => {
    startTransition(() => {
      setInputValueAction(value);
    }
  }
}
```

例えば重いレンダリング処理をラップしてあげることで、 UI がブロックされないようになる（ユーザーの操作をレンダリング処理開始～完了の間も受け入れてくれる）

`isPending` は startTransition の処理が行われてるタイミングから完了までの間 `true` となる。
これが `true` の間はアクションの実行内容がレンダリングされない。
加えて、この間はユーザーが別の操作で上書きを行った場合、それまでの操作を破棄して新しい操作の内容をバックグラウンド実行する。

## 参考

[useTransition – React](https://ja.react.dev/reference/react/useTransition)
