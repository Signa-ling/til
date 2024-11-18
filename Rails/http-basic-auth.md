# Http basic auth

rails でBasic認証を行う場合以下のように書く

```rb
class SampleController < ApplicationController
   http_basic_authenticate_with name: 'hogehoge', password: 'fugafuga' # 一般的には環境変数とかから割り当てる
end
```

RSpec でテストする場合は以下のように書くと通せる

```rb
describe 'サンプル' do
  before do
    name = 'hogehoge'
    password = 'fugafuga'
    authorization = ActionController::HttpAuthentication::Basic.encode_credentials(name, password)

    get("/sample", headers: { 'HTTP_AUTHORIZATION' => authorization })
  end
end
```

## 参考

- [ActionController::HttpAuthentication::Basic](https://api.rubyonrails.org/classes/ActionController/HttpAuthentication/Basic.html)
