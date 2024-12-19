# Given-When-Then Pattern

テストコードを読みやすくするためのプラクティスで、GWT とか呼ばれてる。

ざっくり言うと

- Given: 前提条件のことで、データなどを用意する
- When: アクションのことで、テスト対象の操作などを記述する
- Then: 期待される結果のことで、アサーションを行う

Rspec で書くと多分以下のようになると思う

```ruby
describe 'name のバリデーションについて' do
  # give
  let(:user) { build(:user, name: name) }

  context '名前が無いとき' do
    # when
    let(:name) { nil }

    it 'エラーになること' do
      # then
      expect(user).to be_invalid
    end
  end

  context '名前があるとき' do
    # when
    let(:name) { 'alice' }

    it 'エラーにならないこと' do
      # then
      expect(user).to be_valid
    end
  end
end
```

## 余談

元々 AAA パターンを知っていたが GWT って何が違うんだ？となって調べた。

ちなみに AAA パターンは

- Arrange: 準備
- Act: 実行
- Assert: 確認

のこと。

色々調べてみると言い方が違うだけで、内容に変わりはないっぽい。

ただ、この言い回しが違うことで、 「AAA パターンで表現しづらいけど GWT で考えるとどこに書けばいいかが分かる！」というケースもあるらしい。

覚えておきたい :memo:

## 参考

- [UI のテストは AAA パターンより Given-When-Then パターンの方がしっくりくるかもしれない](https://zenn.dev/m10maeda/articles/gwt-might-feel-more-natural-than-3a-for-ui-testing#given-when-then-%E3%83%91%E3%82%BF%E3%83%BC%E3%83%B3%E3%81%A8%E3%81%AF)
- [c# - Differences between Given When Then (GWT) and Arrange Act Assert (AAA)? - Software Engineering Stack Exchange](https://softwareengineering.stackexchange.com/questions/308160/differences-between-given-when-then-gwt-and-arrange-act-assert-aaa)
