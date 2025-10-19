# Covariance and Contravariance（共変性と反変性）

## 概要

型には変性がある。これは型パラメータを持つ型（関数とか）において、パラメータの部分型関係が外側の型の部分型関係にどう影響するかを表す。

変性には共変性、反変性、不変性、双変性などがあるが、ここでは共変性と反変性について残す。

### 共変性 (Covariance)

関数の戻り値型など、「出力位置」にある型パラメータにおいて、型パラメータの部分型関係と全体の部分型関係が同じ向きになる性質のこと。

関数で例示すると、`A <: B` のとき `() => A <: () => B` も成り立つ。（`A <: B` は「`A` は `B` の部分型」を意味する）

より一般的に示すと、型コンストラクタ `F` において、`A <: B` のとき `F<A> <: F<B>` が成り立つなら、`F` は共変である。

共変な型コンストラクタの例
- 関数の戻り値型: `() => T`
- 非同期の出力: `Promise<T>`
- 読み取り専用配列: `ReadonlyArray<T>`

#### 関数における具体例

2D 座標を返す型 `Get2DPoint` と、その型を引数として受け取って、平面上の原点からの距離を計算する関数 `getDistance2D()` があるとする。

```ts
// 2D座標を返す関数の型
type Get2DPoint = () => { x: number; y: number };

// 平面上の原点からの距離を計算
const getDistance2D = (getPoint: Get2DPoint): number => {
  const p = getPoint();
  return Math.sqrt(p.x ** 2 + p.y ** 2); // √(x^2 + y^2)
};
```

当然、`Get2DPoint` 型の関数 `get2DPoint()` は `getDistance2D()` に渡せる。

```ts
const get2DPoint: Get2DPoint = () => ({ x: 3, y: 4 });
getDistance2D(get2DPoint); // 正常に動作する (結果: 5)
```

ここで、以下のように `Get2DPoint` の返り値を拡張した `Get3DPoint` 型の関数も同様に渡せる。

```ts
type Get3DPoint = () => { x: number; y: number; z: number };
const get3DPoint: Get3DPoint = () => ({ x: 3, y: 4, z: 5 });
getDistance2D(get3DPoint); // これもOK (z座標は無視される)
```

これは `Get3DPoint` 型が `Get2DPoint` の部分型（= `Get2DPoint` が要求すること（x, yを返す）は `Get3DPoint` が提供できる）であり、安全であるため。

型の関係を整理すると以下のように表現でき、これは両方とも `<:` の向きが同じである。つまり、共変性の関係になる。
- オブジェクト型レベル（関数の戻り値型）: `{ x, y, z } <: { x, y }`
- 関数型レベル（関数型全体）: `() => { x, y, z } <: () => { x, y }`

### 反変性 (Contravariance)

関数の引数型など、「入力位置」にある型パラメータにおいて、型パラメータの部分型関係と全体の部分型関係が逆向きになる性質のこと。

関数で例示すると、`A <: B` のとき `(arg: B) => void <: (arg: A) => void` が成り立つ。

より一般的に示すと、型コンストラクタ `F` において、`A <: B` のとき `F<B> <: F<A>` が成り立つなら、`F` は反変である。

反変な型コンストラクタの例
- 関数の引数型: `(arg: T) => void`
- イベントハンドラ: `(event: T) => void`
- 比較関数: `(a: T, b: T) => number`

#### 関数における具体例

3D 座標を受け取る関数の型 `Handle3DPoint` と、その型を引数として受け取って、空間上の点を処理する関数 `processPoint()` があるとする。

```ts
// 3D座標を受け取る関数の型
type Handle3DPoint = (point: { x: number; y: number; z: number }) => void;

// 3D座標を処理する
const processPoint = (handler: Handle3DPoint): void => {
  handler({ x: 3, y: 4, z: 5 });  // 3D座標を渡す
};
```

`Handle3DPoint` 型の関数 `handle3D()` は `processPoint()` に渡せる。

```ts
const handle3D: Handle3DPoint = (p) => {
  const distance = Math.sqrt(p.x ** 2 + p.y ** 2 + p.z ** 2);
  console.log(`3D Distance: ${distance}`);  // √(x^2 + y^2 + z^2)
};
processPoint(handle3D); // 正常に動作する (結果: 3D Distance: 7.0710...)
```

ここで、以下のように `Handle3DPoint` の引数型を縮小した `Handle2DPoint` 型の関数も同様に渡せる。

```ts
type Handle2DPoint = (point: { x: number; y: number }) => void;
const handle2D: Handle2DPoint = (p) => {
  const distance = Math.sqrt(p.x ** 2 + p.y ** 2);
  console.log(`2D Distance: ${distance}`);  // √(x^2 + y^2)
};
processPoint(handle2D); // これもOK (z座標は無視される)
```

これは `Handle2DPoint` 型が `Handle3DPoint` の部分型（= `Handle3DPoint` が渡すもの（x, y, z座標）は `Handle2DPoint` が受け取れる）であり、安全であるため。

型の関係を整理すると以下のように表現でき、これは `<:` の向きが逆になっている。つまり、反変性の関係になる。
- オブジェクト型レベル（関数の引数型）: `{ x, y, z } <: { x, y }`
- 関数型レベル（関数型全体）: `(point: { x, y }) => void <: (point: { x, y, z }) => void`

ここで、逆の関係は成り立たない。 もし共変のように扱おうとすると、型安全性が壊れる。

```ts
type Handle2DPoint = (point: { x: number; y: number }) => void;

const processPoint2D = (handler: Handle2DPoint): void => {
  handler({ x: 3, y: 4 });  // 2D座標のみ渡す
};

const handle3D: Handle3DPoint = (p) => {
  const distance = Math.sqrt(p.x ** 2 + p.y ** 2 + p.z ** 2);
  console.log(`3D Distance: ${distance}`);
};

processPoint2D(handle3D); // ❌ 型エラー（TypeScriptが防ぐ）
// もし許可されると、p.z が undefined となり、計算結果が NaN になる
```

`processPoint2D` は2D座標（`x`, `y` のみ）しか渡さないため、`handle3D` は `p.z` にアクセスできず、`undefined` となってしまう。
これは `{ x, y } <: { x, y, z }` という部分型関係が成り立たないためである。

## 備考

- 不変性、双変性という性質もあるが、ここでは扱わない
  - 読んだ本に記載がなかったため

## 参考

- [型システムのしくみ ― TypeScriptで実装しながら学ぶ型とプログラミング言語](https://www.lambdanote.com/products/type-systems)
  - 7章の範囲で説明あり
- [TypeScript の変性（共変・反変）を 5 分で理解する](https://zenn.dev/jay_es/articles/2024-02-13-typescript-variance)
