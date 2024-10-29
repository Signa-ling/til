# Cookie VS Web Storage

| 種類            | 別タブ/ウィンドウでのデータ共有 | データ容量         | 有効期限                         | サーバーへのデータ送信                   |
| --------------- | ------------------------------- | ------------------ | -------------------------------- | ---------------------------------------- |
| cookie          | ○                              | 4KB                | 指定期限まで有効                 | サーバーへアクセスするたびに毎回自動送信 |
| local storage   | ○                              | 1オリジン当たり5MB | 保存期間の期限なし               | 必要時のみスクリプトやフォームなどで送信 |
| session storage | ×                               | 1オリジン当たり5MB | ウィンドウやタブを閉じるまで有効 | 必要時のみスクリプトやフォームなどで送信 |

以上から、基本的な指針としては容量の大きさと保存期間を軸に決めることになりそう

また、後述するセキュリティの観点からは Cookie を選択したほうが良い

## Cookie に関する補足

主に以下の3用途で用いられる

- セッション管理: ユーザーのログイン状態、ショッピングカート、ゲームのスコア、またはその他のユーザーセッションに関するサーバーが覚えておくべきその他のもの
- パーソナライズ: 表示言語や UI テーマのようなユーザー設定。
- トラッキング: ユーザーの行動の記録および分析

また Cookie を保護するために以下のような属性がある

- 意図しない第三者やスクリプトからのアクセスを防ぐ: `Secure`, `HttpOnly`
- 送信先の定義: `Domain`, `Path`
- サードパーティー Cookie の制御: `SameSite`

## 参考

- [HTTP Cookie の使用 - HTTP | MDN](https://developer.mozilla.org/ja/docs/Web/HTTP/Cookies)
- [ウェブストレージ API - Web API | MDN](https://developer.mozilla.org/ja/docs/Web/API/Web_Storage_API)
- [CheatSheetSeries/cheatsheets/HTML5_Security_Cheat_Sheet.md · OWASP/CheatSheetSerie](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/HTML5_Security_Cheat_Sheet.md#local-storage)
- [CookieとWeb Storageの仕様を比較する](https://zenn.dev/sjbworks/articles/cookie-webstorage)
