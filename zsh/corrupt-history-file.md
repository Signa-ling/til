# Corrupt history file

wsl2 上の ubuntu を起動した際に以下のような文言が出る時があった。

```zsh
zsh: corrupt history file /home/workspace/.zsh_history
```

多分 ubuntu が起動してる状態で windows を終了すると発生するのだと思う。

`.zsh_history` を開いてみると履歴の一部に

```zsh
`@`@`@`@`@`@`@`@`@
```

のような文字列が含まれていた。

これを削除するとエラーは表示されなくなる。
