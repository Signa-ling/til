# Custom Fetch

背景: GraphQL エンドポイントの URL に対し、実行内容に応じて異なるクエリパラメータを投げたかった

公式曰く以下のように書けば良いらしい

```js
const customFetch = (uri, options) => {
  const { operationName } = JSON.parse(options.body);
  return fetch(`${uri}/graph/graphql?opname=${operationName}`, options);
};

const link = new HttpLink({ fetch: customFetch });
```

`HttpLink` には `uri` と `fetchOptions` というものを保持しており、それらを `customFetch()` に渡しているらしい。

`uri` はデフォルトだと `/graphql` が設定されている。また、 `fetchOptions` の持つ内容については[こちら](https://developer.mozilla.org/en-US/docs/Web/API/RequestInit)に記載されているものが該当する。

## 参考

- [HTTP Link - Apollo GraphQL Docs](https://www.apollographql.com/docs/react/api/link/apollo-link-http#customizing-fetch)
- [RequestInit - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/RequestInit)