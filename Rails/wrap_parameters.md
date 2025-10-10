# wrap_parameters

## 概要

Rails の Controller が提供するメソッドで、 Controller 名から Resource 名を推測し `params` を Resource 名でラップする

例えば以下のようなコードがあったとする

```rb
class UsersController < ApplicationController
  def index
    User.create!(user_params)
  end

  private

  def user_params
    params.require(:user).permit(:name, :address)
  end
end
```

`/index` にリクエストとして

```json
{ "user": { "name": "acme", "address": "123 Carrot Street" } }
```

を送ると、 Controller は `params` に以下のデータを含めてくれる

```rb
{ user: { name: "acme", address: "123 Carrot Street" } }
```

---

ここで、以下のように `user` キーが無いデータを送るとする

```json
{ "name": "acme", "address": "123 Carrot Street" }
```

普通に考えると、以下の構造が渡るので `user_params()` が想定した挙動にならない

```rb
{ name: "acme", address: "123 Carrot Street" }
```

ここで、`wrap_parameters` が有効な場合、 リソース名（今回であれば `UsersController` より `user` と推測）でラップした構造にしてくれる

```rb
{ user: { name: "acme", address: "123 Carrot Street" } }
```

なので `user_params()` は想定通りの挙動をしてくれる

## 備考

以下のように設定することで無効化することができる

```rb
class UsersController < ApplicationController
  wrap_paramters false
end
```

また、 `config.action_controller.wrap_parameters_by_default` が有効になっている場合、`params` には元々の構造と `wrap_paramters` によってラップされた構造の両方が含まれるようになる

```rb
{ name: "acme", address: "123 Carrot Street", user: { name: "acme", address: "123 Carrot Street" } }
```

`config.action_controller.wrap_parameters_by_default` のデフォルト値は 7.0 以降は `true`、それ以前は `false` になる

## 参考

- [Action Controller の概要 - Railsガイド](https://railsguides.jp/action_controller_overview.html#wrap-parameters%E3%82%92%E8%A8%AD%E5%AE%9A%E3%81%99%E3%82%8B)
- [Rails アプリケーションの設定項目 - Railsガイド](https://railsguides.jp/configuring.html#config-action-controller-wrap-parameters-by-default)
- [ActionController::ParamsWrapper](https://api.rubyonrails.org/v8.0/classes/ActionController/ParamsWrapper.html)
