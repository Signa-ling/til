# OR 演算子 vs Null 合体演算子

以下のような使い方をする時に、どっちがどういう挙動するのかよく忘れるのでメモ

```javascript
const x = 0;

console.log(x || "falsy");
console.log(x ?? "falsy");
```

ざっくりいうと、

- Null 合体演算子 `??` の方は `null` か `undefined` を falsy な値とする
- OR 演算子は↑に加えて `NaN`, `0`, `""` も falsy な値とする

## 参考

- [論理和 (||) - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Logical_OR)
- [Null 合体演算子 (??) - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing)
