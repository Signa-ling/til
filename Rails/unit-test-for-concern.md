# Unit test for Concern

Concern に class method を書いた際に単体テストしたくなったので調べた

こんな concern があるとして、
```rb
module HogeModule
  extend ActiveSupport::Concern

  class_methods do
    def sample
    end
  end
end
```

`let` と `Class.new {}` で定義可能

あるいは、`stub_const` と `Class.new {}` を組み合わせることで定義できる

```rb
Rspec.describe HogeModule do
  before do
    # テスト用のダミークラスを作る
    stub_const(
      'DummyClass',
      Class.new { include HogeModule }
    )
  end

  describe '.sample_method' do
    subject(:result) { DummyClass.sample_method }
  end
end
```

ちなみに `stub_const` は `before(:all)` の中では動作しない。

また、Concern の単体テストで `RSpec.describe Hoge, type: :concern` のように concern を type 指定してもコケる。（あると思って書いたら無くてコカしまくった）

デフォルトにこの type 指定は無いらしい（自前で定義すれば動くっぽい）

## 参考

- [Method: RSpec::Mocks::ExampleMethods#stub_const — Documentation for rspec/rspec-mocks (main)](https://rubydoc.info/github/rspec/rspec-mocks/RSpec%2FMocks%2FExampleMethods:stub_const)
- [Class.new (Ruby 3.4 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Class/s/new.html)
