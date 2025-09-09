# Image Name

## 概要

docker image 選ぶときに buster とか slim とか色々あって、どれ選べばいいのかよくわからなかったので調べた

どうも大きく分けて4種類あるらしい

### 1. 公式

言語などのバージョンのみが記載されていたらこれ
rust だと

```Dockerfile
FROM rust:1.89.0
```

のように使う場合がこれに相当する

### 2. Alpine Linux

`alpine` と書かれているのは Alpine Linux と呼ばれる Linux ディストリビューションをベースに作成される

記法はいくつかあるが、一例として以下のようなパターンがある

```Dockerfile
# 言語などのバージョン + distribution version
FROM rust:1.89.0-alpine3.21

# distoribution version only
# この場合言語のバージョンは latest, alpine のバージョンも latest に相当する
FROM rust:alpine
```

Alpine Linux は超軽量 + セキュリティ性を売りにしているらしい。

docker におけるイメージボリュームが小さくなるのが強みだが、反面後述する Debian 系の OS に比べると提供パッケージが少なく、開発環境として扱いづらい印象（n=1 の意見）


### 3. Debian

`bullseye` や `buster`, `stretch` などがこれに相当する。

これらは Debian のバージョンのコードネームである。（MacOS でも Sonoma とか Yosemite とかあるけどアレと同じ感じ）

今までに見たのだと下記。今後もバージョンが上がればレパートリーが増えていくはず。

- jessie: v8
- stretch: v9
- buster: v10
- bullseye: v11
- bookworm: v12

### 4. WindowsServer

自分は見たことがなかったのだが、 docker container で Windows のコンテナイメージが提供されているらしい…。

他の記事などで話が出ているのを見ただけで実際にそのタグがついたコンテナイメージは見たことがないので、説明は割愛。

## 参考

- [Docker imageの種類・選び方](https://zenn.dev/ken3pei/articles/1abbf7d974cf5d)
- [rust - Official Image | Docker Hub](https://hub.docker.com/_/rust)
- [Debianのバージョン履歴 - Wikipedia](https://ja.wikipedia.org/wiki/Debian%E3%81%AE%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E5%B1%A5%E6%AD%B4)
