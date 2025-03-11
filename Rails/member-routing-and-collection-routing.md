# Member routing and Collection routing

Rails のルーティングにはメンバールーティングとコレクションルーティングが存在する。

すごく平たくいうと以下の通り

| ルーティング区分         | 概要                                    | 例                   |
| ------------------------ | --------------------------------------- | -------------------- |
| メンバールーティング     | id で指定した単一要素の操作に関わるもの | show, update, delete |
| コレクションルーティング | 複数のメンバーの操作に関わるもの        | index                |

## 指定方法

ブロックで渡すパターンと `:on` オプションで指定するパターンがある。

以下はメンバールーティングに対してそれぞれのパターンで書いた例

```rb
resources :sample do
  # ブロックの場合
  member do
    get 'foo'
  end

  # :on オプションの場合
  get 'bar', on: :member
end
```

## 参考

- [Rails のルーティング - Railsガイド](https://railsguides.jp/routing.html#restful%E3%81%AA%E3%83%AB%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0%E3%82%92%E3%81%95%E3%82%89%E3%81%AB%E8%BF%BD%E5%8A%A0%E3%81%99%E3%82%8B)
