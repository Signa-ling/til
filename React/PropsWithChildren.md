# PropsWithChildren

children を受け取る時に Props に明示しなくてもよくなる

```tsx
// 使わない場合
type Props = {
  children: ReactNode;
}

const SampleComponent = ({ children }: Props) => {
  // ...
}

// 使う場合
import { PropsWithChildren } from "react";

const SampleComponent = ({ children }: PropsWithChildren) => {
  // ...
}
```

他の Props を受け取る場合は

```tsx
import { PropsWithChildren } from "react";

type Props = {
  text: string;
};

const SampleComponent = ({ children, text }: PropsWithChildren<Props>) => {
  // ...
}
```

のように `PropsWithChildren<Props>` みたいな形式で書く。

## ポエム

型が見辛いかも？と思ったりしたが、慣れればそうでもないのだろうか？

個人で使うにはいいけど、チームで使うならそういう面で可読性とかを気にして採用すべきか判断するのが良いのかも。

## 参考

- [PropsWithChildren vs ReactNode in TypeScript | by Zar Nabi | Medium](https://medium.com/@colorsong.nabi/propswithchildren-vs-reactnode-in-typescript-c3182cbf7124)
