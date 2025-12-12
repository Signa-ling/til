# After Initialize Overridden by FactoryBot

## 概要

新規作成画面 (`/new`) で初期値をデータ保存前にも表示できるよう、`after_initialize` を使って `new` 時点でデフォルトタイトルを設定する処理を書いた。

```rb
class Document
  after_initialize :set_default_title

  field :title
  field :document_type  # 'report', 'proposal', 'contract'

  private

  def set_default_title
    return if persisted?
    self.title ||= "#{document_type.titleize} - #{Time.current.strftime('%Y%m%d%H%M%S')}"
  end
end
```

これのコールバック関数についてのテストを書いた。

```rb
describe 'set_default_title callback' do
  let(:document) { build(:document, document_type: 'report') }

  it 'タイトルが自動生成されること' do
    expect(document.title).to match(/\AReport - \d{14}\z/)
  end
end
```

が、実際には `title` は別の値に上書きされていた。
上書きしていた値は Factory で定義されていた値だった。

```rb
factory :document do
  title { 'テスト文書' }
  document_type { 'report' }
end

# 以下の expect では 'テスト文書' が表示されていた
expect(document.title).to eq('テスト文書')
```

調べてみると [FactoryBot 内の処理](https://github.com/thoughtbot/factory_bot/blob/main/lib/factory_bot/attribute_assigner.rb#L13-L21)のせいでそのようになっていたらしい。

```rb
def object
  @evaluator.instance = build_class_instance # ← ここで new される (after_initialize の発火)
  build_class_instance.tap do |instance|
    attributes_to_set_on_instance.each do |attribute|
      instance.public_send(:"#{attribute}=", get(attribute)) # ← FactoryBot で定義した初期値や build(:document, title: '手動設定') のように override した値が入る
      @attribute_names_assigned << attribute
    end
  end
end
```

回避策として思い浮かんだのだと以下の2つ

1. `described_class.new` を使う
2. `transient` を使う

### 1. `described_class.new` を使う

```rb
let(:document) { described_class.new(document_type: 'report') }
```

みたいな感じで定義することで `after_initialize` の挙動をそのままテストできる。
既存のテストに影響しないので安全。

### 2. `transient` を使う

```rb
factory :document do
  document_type { 'report' }

  transient do
    title { nil }
  end

  after(:build) do |document, evaluator|
    # Factory の呼び出し側で値が指定されていれば上書きを行う
    # 指定されて無ければ after_initialize の値のまま
    if evaluator.title
      document.title = evaluator.title
    end
  end
end

# 呼び出し側
build(:document, document_type: 'report') # => 'Report - 20241210123456' (callback)
build(:document, document_type: 'report', title: '議事録') # => '議事録' (明示指定)
```

Factory の全体設計を見直すならこっちも選択肢としてあり

## 参考

- [factory_bot/lib/factory_bot/attribute_assigner.rb at main · thoughtbot/factory_bot](https://github.com/thoughtbot/factory_bot/blob/main/lib/factory_bot/attribute_assigner.rb#L13-L21)
