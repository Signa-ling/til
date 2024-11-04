# Why use React FC?

普段業務では `JSX.Element` を使っていたためこれに則っていたが、`React.FC` を使うかどうかの議論を見かけたので、自分の中でどう使い分けるべきかを考えることにした。

## 結論

- 基本は`React.FC` を使う
- 戻り値を厳密にしたい場合やジェネリクスを使う必要がある場合、使わなくても良い
- ただし eslint などでエラーが出る場合はそちらを優先する（チームのルールになっている気がするので）

とするのが良さそう。

## FC の嬉しいポイント

Props と戻り値の型を一発で定義できることだと思う。

```tsx
type Props = {
  name: string;
};

const Sample: React.FC<Props> = ({ name }) => <div>{name}</div>; // 戻り値の型が不要
```

例えば他の書き方として定番の `JSX.Element` だと以下のように書く。

```tsx
type Props = {
  name: string;
};

const SampleV2 = ({ name }: Props): JSX.Element => <div>{name}</div>;
```

この場合、Props とは別に都度 `JSX.Element` を戻り値として記述する必要があり、冗長な感じがする。

また、引数部分のカッコ内 `({name}: Props)` の可読性が少し悪いとも感じている。この点を減らせるのも嬉しい。

## FC vs JSX.Element

`React.FC` を使わずに `JSX.Element` を使うことがある。

この書き方は2023年に非推奨となっており、今後は `React.JSX.Element` を使う必要がある。

加えて戻り値の型には他にも `React.ReactNode` や `React.ReactElement` などを選ぶことができ、どれを選択すべきなのがを考える必要がある。

こうした戻り値を厳密にしたいのであれば `React.FC` を使わないほうが良いが、厳密にしなくても良いのであれば迷うコストを減らせる `React.FC` を使う方が望ましいと思う。

## FC vs VFC

`React.FC`が使われない理由の一つとして、使うつもりがない `children` が自動で許容されており、バグを含む可能性が生じる点があること。

これを許可しないのが `React.VFC` なのだが、これはv18から非推奨となる。そしてv18以降は `React.FC` は従来の `React.VFC` と同等の挙動をする。

```tsx
// React v18 以降は下記のコードは動かない
import * as React from "react";

const Sample: React.FC = ({ children }) => <div>{children}</div>; // will error with "Property 'children' does not exist on type '{}'.
```

よって、VFCは避け、FC を使うほうが望ましそうである。

## ジェネリクスどうすんの？

欠点について話したので、他の欠点についても関連して述べる。

`React.FC` の他の欠点という形で語られるものとして、ジェネリクスが使えない。

が、これに対して「ジェネリクスを使う場合だけ `React.FC` を避ければ良い」という考えを持ってる人を見かけた。
自分としては、この考えは自分としては賛同できた。固執する必要はないと思う。

## 備考

性能面についてはもう少し深堀らないといけない。

## 参考

- [React.FCを使うよ、理に適っているからね | stin's Blog](https://blog.stin.ink/articles/i-do-use-react-fc-type)
- [コンポーネントに型をつけるときFCを使うかJSX.Elementを使うか | tatsumiyamamoto.com](https://tatsumiyamamoto.com/posts/4fd009ec41f4d6)
- [RFC: Remove return types from React components · Issue #2735 · skeletonlabs/skeleton](https://github.com/typescript-cheatsheets/react/issues/643)
- [Remove React.FC from Typescript template by Retsam · Pull Request #8177 · facebook/create-react-app](https://github.com/facebook/create-react-app/pull/8177)
