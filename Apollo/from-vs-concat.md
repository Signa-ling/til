# from vs concat

Apollo Link でリンクを繋げる方法が、大きく2種類ある。

書き方はそれぞれ以下の通り

```typescript
// from pattern
const link = from([
    errorLink,
    authLink,
    httpLink
])

// concat pattern
const link = errorLink.concat(authLink.concat(httpLink))
```

結果的に上記はどちらも同じ形になるっぽい。なので、可読性を鑑みて選ぶのが良さそう。2個までなら `concat`, それ以上なら `from` を使うかなあ…

また、どちらを使うにしても、末尾は terminating link となる `HttpLink` （GraphQL サーバーと通信するための link）を置かないと駄目らしい。

## 参考

- [Apollo Client の ApolloLink チェーンで HTTP リクエストをカスタマイズする｜まくろぐ](https://maku.blog/p/xa62yo4/#link-%E3%83%81%E3%82%A7%E3%83%BC%E3%83%B3%E3%82%92%E6%A7%8B%E7%AF%89%E3%81%99%E3%82%8B)