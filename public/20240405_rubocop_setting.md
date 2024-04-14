---
title: RailsチュートリアルにRubocopを導入
tags:
  - Rails
  - Railsチュートリアル
private: false
updated_at: '2024-04-14T10:39:05+09:00'
id: 436dcd3abb09929bf2b5
organization_url_name: manabi-labo
slide: false
ignorePublish: false
---

こんにちは！学びと成長しくみデザイン研究所の斉藤です。
本記事はRailsチュートリアルの第14章が終わった段階の[リポジトリ](https://github.com/yasslab/sample_apps/tree/main/7_0/ch14)をベースにして、より実践的なものへカスタマイズしていきます。

- カスタマイズ履歴
    - [letter_opener_web 導入](https://qiita.com/SaitoJP/items/f86085e6f5580b803a9f)
    - RuboCop 導入 <= 今ココ

前回から引き続き、今回は RuboCop を導入します。
RuboCop を導入することでコード整形が自動化され、作業効率が向上します。
※ RuboCop の概要については公式サイトをご確認ください: https://docs.rubocop.org/rubocop/index.html#overview


# 導入手順


## RuboCop の導入

### Gemfileへ追加

開発環境に限定してRuboCopを導入します。
※ Rails起動時(bin/rails s)した際にはRuboCopの gem を require する必要が無いため、`require: false` を指定します。

```rb:Gemfile
group :development do
  gem 'rubocop', '1.62.1', require: false
  gem 'rubocop-performance', '1.21.0', require: false
  gem 'rubocop-rails', '2.24.1', require: false
  # 以下省略
end
```


### ライブラリのインストール

※ ちなみに `bundle install` は `bundle` に省略可能です。
```console
bundle
```


### RuboCopによる自動修正1

RuboCopを使ってコードの自動修正を行います。
まずは安全な自動修正から

```console
bundle exec rubocop -a
```

念の為、修正された箇所が正常に動作するか確認します。
```console
bin/rails test
```

テストが通ったのでgitでコミット

### RuboCop の設定ファイル作成

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
    - 'Guardfile'

# 日本語のコメントを許可する
Style/AsciiComments:
  Enabled: false

# class,moduleのドキュメントは必須にしない
Style/Documentation:
  Enabled: false
```


### RuboCopによる自動修正2

次に、より多くの自動修正を行うオプションを使用しますが、この際には修正後の動作を十分に確認してください。
(今回の場合は単体テストが整備されているのでテストが通れば問題ありません)
```console
bundle exec rubocop -A
```

再度、`bin/rails test` を実行して、問題なければgitコミットします。

### 自動修正されなかった箇所を修正


`bundle exec rubocop` を実行すると、いくつか `C` (Convention)が表示されるため、内容に従って修正します。
修正後はまたテストを実行してください。


### さいごに

今後、`bundle exec rubocop` を実行した際、警告やエラーが表示された箇所を修正することで、
プロジェクト全体として一貫したコードフォーマットを維持することができます。

## VSCode上でRuboCopを実行する設定

エディタ上でRuboCopを実行することにより、リアルタイムにフォーマットが適用され作業効率が上がります。

1. RuboCop の拡張機能をインストール

    以下を開いて `Install` ボタンを押します。
    https://marketplace.visualstudio.com/items?itemName=rubocop.vscode-rubocop

1. VSCode上でRuboCopの設定

    VSCodeの `settings.json` を開いて以下を追記することで、
    ファイルを保存した際にRuboCopのフォーマットを自動適用します。

    ```json:settings.json
    "[ruby]": {
      "editor.formatOnSave": true,
      "editor.defaultFormatter": "rubocop.vscode-rubocop"
    }
    ```
