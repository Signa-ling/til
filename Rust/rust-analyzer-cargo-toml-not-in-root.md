# Rust Analyzer Cargo Toml Not In Root Error Avoidance

## 概要

Rust の LSP である rust-analyer の VSCode 拡張機能は、ルートディレクトリにある `Cargo.toml` を読み込もうとする。

ただ、開発ケースによってはルートディレクトリに無いケースもある（例えばモノレポ構成）

そういうときは `.vscode/settings.json` でプロジェクトの `Cargo.toml` のあるパスを指定する

```json
{
  "rust-analyzer.linkedProjects": [
    "childDir/cargoProjectName/Cargo.toml"
  ]
}
```

## 参考

- [VSCode setting to specify path to Cargo.toml · Issue #2649 · rust-lang/rust-analyzer](https://github.com/rust-lang/rust-analyzer/issues/2649)
