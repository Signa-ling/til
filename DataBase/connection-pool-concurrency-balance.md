# Connection Pool Concurrency Balance

## 概要

マルチスレッド/マルチプロセスで動作するアプリケーションがデータベースに接続する場合、同時実行数（スレッド数やプロセス数の合計） ≤ データベースのコネクションプールサイズにするのが望ましい

理由は、同時実行数がプールサイズを超えるとコネクション取得待ちが発生し、遅延やタイムアウトを引き起こすため

## 余談

調べたきっかけは、ローカル環境で Sidekiq 動かそうとしたら Sidekiq の実行数 > DB 接続のプールサイズになっていて、取得待ちによるボトルネックが発生していたため

ローカル環境上でプールサイズを上回るほどの並列作業をしたことがなかったことと、本番側でもここを意識した開発に取り組むことがなかったので、初めてこの手のボトルネックがあることを気にする機会になった。

## 参考
- [Database Connection Pooling (Rails Guide)](https://guides.rubyonrails.org/configuring.html#database-pooling)
- [General Database Performance Tips](https://use-the-index-luke.com/)
