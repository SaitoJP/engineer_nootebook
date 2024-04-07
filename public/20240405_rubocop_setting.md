---
title: RailsチュートリアルにRubocopを導入してみた
tags:
  - Rails
  - Railsチュートリアル
private: true
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: true
---

こんにちは！学びと成長しくみデザイン研究所の斉藤です。
本記事はRailsチュートリアルの第14章が終わった段階の[リポジトリ](https://github.com/yasslab/sample_apps/tree/main/7_0/ch14)をベースにして、より実践的なものへカスタマイズしていきます。
以下、カスタマイズ履歴です。

- カスタマイズ履歴
    - letter_opener_web 導入

前回から引き続き、今回は rubocop を導入します。
rubocopを導入することでコード整形が自動化され作業効率が上がります。


# 導入手順


## rubocop を導入

Gemfileにrubocop追加
※ rails起動時にrequireの必要がないため、`require: false` をつける
```rb:Gemfile
group :development do
  gem 'rubocop', '1.62.1', require: false
  gem 'rubocop-performance', '1.21.0', require: false
  gem 'rubocop-rails', '2.24.1', require: false
  # 以下省略
end
```


ターミナルで bundle install
```console
bundle
```

rubocop の自動修正を実行
```console
bundle exec rubocop -a
```

念の為、修正された箇所が正常に動作するか確認
```console
bin/rails test
```

テストが通ったのでgitでコミット


プロジェクト直下に rubocop の設定ファイルを作成
```yml:.rubocop.yml
require:
  - rubocop-rails
  - rubocop-performance

AllCops:
  NewCops: enable
  TargetRubyVersion: 3.2
  Exclude:
    - 'node_modules/**/*'
    - 'vendor/**/*'
    - 'bin/**/*'
    - 'config/**/*'
    - 'db/**/*'
    - 'tmp/**/*'

# 日本語のコメントを許可する
Style/AsciiComments:
  Enabled: false

# class,moduleのドキュメントは必須にしない
Style/Documentation:
  Enabled: false
```


rubocop の挙動が変わってしまう可能性を含んだ自動修正を実行
```console
bundle exec rubocop -A
```

再度、`bin/rails test` を実行して、問題なければgitコミットする


## git hook を使用して、commit 時に rubocop を実行させる


pre-commit を導入することで、gitコミットした際に rubocop が自動的に実行されるようにします。

```sh:.git/hooks/pre-commit
#!/bin/sh

# ステージングに追加されたRubyファイルを検出
FILES=$(git diff --cached --name-only --diff-filter=ACM | grep '\.rb$')

# Rubyファイルがなければ、hookを終了
if [ -z "$FILES" ]; then
  exit 0
fi

# bundle exec rubocop を実行
echo "Running RuboCop on staged Ruby files..."
# 以下の `~/.rbenv/shims/bundle` のパスは適宜変更
~/.rbenv/shims/bundle exec rubocop $FILES

# RuboCopの結果をチェック
if [ $? -ne 0 ]; then
  echo "RuboCop detected issues. Please fix them before committing."
  exit 1
fi

exit 0
```

# さいごに

