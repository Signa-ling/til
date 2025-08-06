# add-til

TIL（Today I Learned）の新規エントリーを作成するコマンド

## 使用例

```
/add-til Rubyのmethod_missingについて
/add-til TypeScriptの型ガードについて
```

## プロンプト

以下のTILエントリーを作成してください：

入力内容を解析して：
1. 言語/技術名を特定（例：Ruby、TypeScript、React等）
2. トピック名を特定して適切なファイル名を生成（snake-kebab形式）

処理手順：
1. 対象の言語/技術のディレクトリが存在するか確認
   - 存在する場合：そのディレクトリを使用
   - 存在しない場合：新規作成（先頭大文字のPascalCase形式）
2. ファイル名を生成（例：method-missing.md、type-guard.md）
3. 以下のテンプレートでmdファイルを作成：

```markdown
# [タイトル（スペース区切りの適切な形式）]

## 概要


## 備考


## 参考

```

4. 作成したファイルのパスをユーザーに通知
5. 必要に応じてgit addを実行

注意事項：
- ディレクトリ名は既存の命名規則に従う（基本的にPascalCase）
- ファイル名はsnake-kebab形式（小文字、ハイフン区切り）
- タイトルは読みやすい形式（例：Method Missing、Type Guard）