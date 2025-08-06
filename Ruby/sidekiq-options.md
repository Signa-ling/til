# Sidekiq Options

Sidekiq に触れてて知ったオプションを記載する

## Queue Option

コマンドライン上で `$ sidekiq -q hoge,2` あるいは以下のように書くオプション

```yml
# config/sidekiq.yml
:queues:
  - [hoge, 2]
```

Sidekiq はデフォルトだと `default` を扱うが、この設定によって名前付きキューを作成できる（嬉しさは後述）
上記例における `2` の数値の部分は優先度で、この値が大きいほど優先して中身を処理してくれる（厳密にはキューを確認する頻度が多くなる）

名前付きキューが設定できることで、

- 優先度の制御
- 特定のワーカーに対応した専用のキューの用意
- 処理の重さや種類に応じてキューを分ける
- 監視やデバッグのしやすさ
- 単一のキューでの問題を他のキューに影響させない

といった嬉しさがある

## 備考

- 学んだら追記する

## 参考
- [Advanced Options · sidekiq/sidekiq Wiki](https://github.com/sidekiq/sidekiq/wiki/Advanced-Options)
