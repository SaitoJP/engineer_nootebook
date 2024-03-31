---
title: Railsチュートリアルのメールプレビューを見やすくした(gem letter_opener_web)
tags:
  - 'Rails'
  - 'Railsチュートリアル'
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

こんにちは！学びと成長しくみデザイン研究所の斉藤です。
Railsチュートリアルのメールプレビュー画面が見づらく使い勝手が悪いので、
`letter_opener_web` を導入して改善してみました。

# プレビュー画面(Before => After)

## Before

![1](https://raw.githubusercontent.com/SaitoJP/engineer_nootebook/main/images/202403/20240331_001.png)

## After

![2](https://raw.githubusercontent.com/SaitoJP/engineer_nootebook/main/images/202403/20240331_002.png)

- メリット
    - URLが `http://localhost:3000/letter_opener` でシンプルでわかりやすい
    - 複数のメールプレビューを確認できる


# 導入手順

gem追加
```ruby:Gemfile
group :development do
  gem 'letter_opener_web', '2.0.0' # <= 追加
  gem "web-console",         "4.2.0"
  gem "solargraph",          "0.50.0"
  gem "irb",                 "1.10.0"
  gem "repl_type_completor", "0.1.2"
end
```

`config/routes.rb` にmount
```ruby:routes.rb
Rails.application.routes.draw do
  mount LetterOpenerWeb::Engine, at: '/letter_opener' if Rails.env.development?

# 〜 以下省略 〜
```

`config/environments/development.rb` に設定追加
```ruby:development.rb
# 以下3行はすでに記載されている設定
host = 'localhost:3000'
config.action_mailer.default_url_options = { host:, protocol: 'http' }
config.action_mailer.perform_caching = false

config.action_mailer.delivery_method = :letter_opener_web # <= 追加
```
