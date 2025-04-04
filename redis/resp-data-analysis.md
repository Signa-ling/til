# RESP data analysis

## 背景

- Upstash から export したデータを用いた分析がしたかった
- export したデータの形式が [RESP (Redis Serialization Protocol)](https://redis.io/docs/latest/develop/reference/protocol-spec/) で、そのままでは解析できなかった
  - 例えばこれが JSON とかならすぐ解析できるわけだけど…

## やったこと

大きく以下の 2 step を実施

- ローカルの redis に RESP の情報を挿入
- redis に対しアクセスできるライブラリを持つ言語を利用して集計

### ローカルの redis に RESP の情報を挿入

1. redis サーバを立てる（今回は使い捨てだったので即席でコンテナを用意した）

```bash
$ docker run --name temp-redis -p 6379:6379 -d redis
```

2. RESP 形式のデータを redis にロード

```bash
$ docker cp ./redis.dump temp-redis:/redis.dump # redis.dump が RESP 形式
$ docker exec -it temp-redis bash # コンテナ外から操作もできるが今回は中に入ったので
$ cat /redis.dump | redis-cli --pipe -h 127.0.0.1 # 読み込み
```

3. ロードできてるかの確認

```bash
# コンテナ内で以下を叩いた。 key 名が返って来れば OK
$ redis-cli KEYS *
```

### redis に対しアクセスできるライブラリを持つ言語を利用して集計

ruby を使った例を示す

```rb
require 'redis'

redis = Redis.new(host: 'localhost', port: 6379)
keys = redis.keys('*')

# 例えば key: [{ message: 'aaa' }, { message: 'bbb' }] という形式でデータを保存してるとする
# ここから message の最大文字数を知りたい場合以下のように書く
messages = []
keys.each do |key|
  objects = redis.lrange(key, 0, -1)

  objects.each do |obj|
    messages << obj['message']
  end
end

puts("最大文字数: #{messages.max}")
```

## 参考

- [Redis serialization protocol specification | Docs](https://redis.io/docs/latest/develop/reference/protocol-spec/)
- [redis/redis-rb: A Ruby client library for Redis](https://github.com/redis/redis-rb)
