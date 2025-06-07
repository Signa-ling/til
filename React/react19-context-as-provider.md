# React19 Context as Provider

React19 から `<Context>` を `<Context.Provider>` の代わりに扱えるようになった。
つまりこんな感じで書ける。

```tsx
const ThemeContext = createContext('');

function App({children}) {
  return (
    // <Theme.Provide value> みたいな書き方をしなくてよくなった
    <ThemeContext value="dark">
      {children}
    </ThemeContext>
  );
}
```

将来的に `<Context.Provider>` は非推奨となるため、既存で `<Context.Provider>` を使っている場合は変更対応が必要になる。
ただ、置き換えのための codemod を提供する予定とのことなので、安全に行えそうである。

## 参考

- [React v19 – React](https://react.dev/blog/2024/12/05/react-19#context-as-a-provider)
