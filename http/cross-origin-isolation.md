# Cross-Origin Isolation

クロスオリジン分離はウェブページで一部の強力な機能を安全に利用するためのセキュリティ機構のこと。

この状態にするには、以下の2つの HTTP ヘッダーを Web ページに設定する必要がある

- Cross-Origin-Opener-Policy(COOP)
- Cross-Origin-Embedder-Policy(COEP)

## Cross-Origin-Opener-Policy

最上位のドキュメントが他のオリジンのドキュメントと閲覧コンテキストのグループを共有していないことを保証する。

ウェブページでは自由に他のページを開いたり、リンクをクリックしたり、ポップアップウィンドウの作成が行える。一方で、他のオリジンのページと同じブラウザコンテキストを共有することで、悪意あるページがユーザーの情報を盗んだり、ブラウザのタブ間などでデータ漏洩を引き起こす危険性がある。

COOP によって制限することでこうしたリスクを防ぐ事ができる。

設定は以下の通り

- `unsafe-none`: デフォルト設定。何も制限しない（上記危険性あり）
- `same-origin-allow-popups`: ポップアップウィンドウは他オリジンも許可するが、それ以外は同一オリジンのみ許可する
- `same-origin`: 同一オリジンのみ許可（最も厳格）

## Cross-Origin-Embedder-Policy

有効にすることでドキュメントが読み込む外部リソースのうち、明示的に許可されたもののみを受け入れるようにする。これによって、意図しないデータ漏洩などセキュリティリスクを軽減する。

設定は以下の通り

- `unsafe-none`: COEP の制限を無効にする（デフォルト設定）
- `require-corp`: 同一オリジンまたは CORP(Cross-Origin Resource Policy)ヘッダーで許可されたオリジンからのみ読み込むことができる

## 参考

- [Cross-Origin-Opener-Policy - HTTP | MDN](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy)
- [Cross-Origin-Embedder-Policy - HTTP | MDN](https://developer.mozilla.org/ja/docs/Web/HTTP/Headers/Cross-Origin-Embedder-Policy)
