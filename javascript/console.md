# console

`console.log()`をよく使うが他にも色々な書き方があるよ～というのを知ったのでメモ

| メソッド名 | 挙動とかの説明 |
| - | - |
| `console.error()` | メッセージが赤色で出力され目立つようになる。主にエラーメッセージとして出力したい場合に使う。 |
| `console.warn()` | メッセージが黄色で出力され目立つようになる。主に警告メッセージとして出力したい場合に使う。 |
| `console.info()` | ログメッセージの分類に役立つが、挙動としては `console.log()` と同じ。 |
| `console.assert()` | 第1引数が false であればエラーメッセージを出力する。 |
| `console.debug()` | デバッグ目的で使用するもので、多くのブラウザのコンソール設定ではデフォルトで非表示になっている。 |
| `console.table()` | データをテーブル形式で表示するもので、配列やオブジェクト操作時に役立つ。 |
| `console.time()`, `console.timeEnd()` | コードの実行にかかる時間を測定するために使う。`time`で計測を開始し、`timeEnd`で計測を止める。 |
| `console.group()`, `console.groupEnd()` | 関連メッセージを読みやすくするために字下げによるグルーピングを行う。`group` を使うことで出力時に字下げを行うことができ、`groupEnd` で現在の `group` を終了する。 |

他にもあるけど、普段使ったことが無くて気になったのだけを抜粋

## 参考

- [console - Web API | MDN](https://developer.mozilla.org/ja/docs/Web/API/console)
