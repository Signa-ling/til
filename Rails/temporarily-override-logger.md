# Temporarily Override Logger

忘れてしまったのでメモも兼ねて残しておく。

Rails でアプリケーション全体で設定しているログの出力先とは別に、特定のログだけ別の場所に流したい場合に使えそうなやり方

```ruby
def logging_by(path, message)
  origin_logger = Rails.logger

  # ログの出力先を変更して書き込む
  Rails.logger = Logger.new(path)
  logger = Rails.logger
  logger.info(message)

  # アプリケーション全体で設定しているログ出力先に戻す
  Rails.logger = origin_logger
end
```

もっといいやり方はあると思うけど、簡易的に済ませるならこれくらいでいい気がする。
