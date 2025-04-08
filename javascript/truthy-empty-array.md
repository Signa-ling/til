# Truthy empty array

javascript における空配列は truthy なのだが、 falsy だと思い込んでた。

```javascript
const arr = [];
console.log(!!arr); // -> true
```

なので、以下のように空配列や falsy な値が返ってくるケースでは判定を複数用意する必要がある。

```typescript
// 型が分かりやすいように ts で記述
const messages: string[] | undefined = getMessages();

// 配列内に要素が無い場合は return
if (!messages || messages.length === 0) return;
```
