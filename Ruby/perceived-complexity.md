# Perceived Complexity

Rubocop の警告の一つで、あるメソッドの複雑さが規定値を超えると発される。

ここでいう複雑さとは分岐の多さを示しており、対象となるのは `if`, `elsif`, `else`, `case`, `when` である。

以下は計算ロジック

```ruby
def complexity_score_for(node)
    case node.type
    when :case
      # If cond is nil, that means each when has an expression that
      # evaluates to true or false. It's just an alternative to
      # if/elsif/elsif... so the when nodes count.
      nb_branches = node.when_branches.length + (node.else_branch ? 1 : 0)
      if node.condition.nil?
        nb_branches
      else
        # Otherwise, the case node gets 0.8 complexity points and each
        # when gets 0.2.
        ((nb_branches * 0.2) + 0.8).round
      end
    when :if
      node.else? && !node.elsif? ? 2 : 1
    else
      super
    end
  end
end
```

ざっくり node.type には `:case`, `:if`, それ以外が渡ってくるっぽく、`:case`, `:if` がそれぞれこのメソッドにおける計算対象になるらしい 

最初にシンプルな `:if` のパターンを見てみると、、 `else` なら 2pt, `if`, `elsif` なら 1pt として扱われるようだ。

```ruby
# つまり以下のような計算になる
def sample(price)
  if price.zero? # ここで 1pt
  elsif price.positive? # ここも 1pt
  else # ここは 2pt
  end
end

# => 上記より、sample method の Perceived Complexity は 4pt
```

次に `:case` のパターンを見てみる。

算出は条件に応じて異なるが、どちらも case の中の when 節と else 節の合計個数が必要になるため、先にその計算を行っている。

算出条件は `node.condition` が `nil` かどうかによる。ここでいう `node.condition` が `nil` とは以下のような状態を指す。

```ruby
case # ここで条件を書かない
when num.zero?
when num.positive?
end
```

この状態は `if/elsif` な状態と同義であると考えて、節の個数（ = `nb_branches`）を返す。

一方で `node.condition` が `nil` でない場合、`case` を 0.8pt, 節1個につき 0.2pt を返し、その合計値を小数点以下で四捨五入する。なので、 `(nb_branches * 0.2) + 0.8).round` pt となる。

```ruby
# つまり以下のような計算になる
def sample2(char)
  case char # これが 0.8pt
  when 'hoge' # これが 0.2pt
  when 'fuga' # これも 0.2pt
  else # これも 0.2pt
  end
end

# => 上記より、sample2 method の Perceived Complexity は 1.4 の小数点四捨五入で 1pt
```

## 余談

今の自分が所属しているチームではこれは導入していない。

他チームの話を輪読会で聞いた際にこの指標について少し触れられたので調べた。

## 参考

- [Metrics :: RuboCop Docs](https://docs.rubocop.org/rubocop/cops_metrics.html#metricsperceivedcomplexity)
- [rubocop/lib/rubocop/cop/metrics/perceived_complexity.rb at master · rubocop/rubocop](https://github.com/rubocop/rubocop/blob/master/lib/rubocop/cop/metrics/perceived_complexity.rb#L36)