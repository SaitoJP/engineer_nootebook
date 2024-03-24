---
title: Railsのおすすめボイラープレート
tags:
  - 'Rails'
private: false
updated_at: ''
id: null
organization_url_name: manabi-labo
slide: false
ignorePublish: false
---

こんにちは！学びと成長しくみデザイン研究所の斉藤です。
みなさんはRails環境での技術検証をしたいとき、手っ取り早く試せるRails環境を作りたいと考えたことはありませんか？
例えば、「gemの動作検証をしたい」や「古いRailsの挙動を調査したい」など。
`rails new` で一から環境構築するのは少し手間だし、どうしようかと思っていたところ、素晴らしいOSSを見つけたのでご紹介します。

## オススメ1

https://github.com/yasslab/sample_apps

Railsチュートリアルのサンプルコード集です。ボイラープレートとしても活用できます。

- おすすめポイント
    - Railsの古いバージョンから新しいものまで幅広くカバーしている
    - 癖のあるgemが導入されておらず、素のRailsに近い構成
- 補足
    - DBがSQLiteなので、本番環境のDBと合わせたい場合は以下の記事を参考にどうぞ
        - https://qiita.com/SaitoJP/items/d0df0ff431ee184b895b

## オススメ2

https://github.com/mattbrictson/nextgen

`rails new` よりも多機能な環境構築ツールです。
DBやJSビルドツールなど様々な構成を自動的に設定してくれます。

- おすすめポイント
    - 最新のRailsバージョンに対応している
    - 豊富な構成オプションがある
- 補足
    - READMEに記載されている `Requirements` を見落とさないこと（特にYarnが必要な点）


# まとめ

今回ご紹介したボイラープレートやツールを使うことで、開発環境のセットアップ時間を削減でき、様々な技術検証を容易に実施できます。
ぜひこの機会に活用し、より快適な開発環境を実現してみてください。
