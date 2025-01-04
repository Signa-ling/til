# Custom Instructions

Github Copilot では Custom Instructions を定義することで、 VSCode などで Copilot を使ってる際にチームのコーディングルールなどを反映したレスポンスを返してくれるようになる

基本的にやることとしては

1. `.github` ディレクトリ内に `copilot-instructions.md` ファイルを作成
2. 1 で作成した markdown ファイルに対応してほしい内容を自然言語で記載

の2ステップだけ

これによって、例えば以下の記述を `.github/copilot-instructions.md` に書いて、これに反したコードの修正を行うことができる

```.github/copilot-instructions.md
# コーディングガイドライン

- 関数宣言はアロー関数を使うこと
- 変数名はキャメルケースを使うこと
- JSDoc を記述すること
```

![動作例](assets/custom-instructions-sample.gif)

## 感想

チーム内でのスタイルガイドをバージョン管理出来るという副次的効果もあり、個人的には大分嬉しさを感じた

## 参考

- [GitHub Copilot のカスタム指示の追加 - GitHub Docs](https://docs.github.com/ja/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot)
- [GitHub Copilot in VS Code でカスタムインストラクションを利用可能になりました | DevelopersIO](https://dev.classmethod.jp/articles/custom-instructions-now-available-in-github-copilot-in-vs-code/)
- [上記記事の内容（前述された動作例）を試せるリポジトリ](https://github.com/Signa-ling/custom-instructions-sample)