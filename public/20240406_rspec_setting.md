---
title: Railsチュートリアルの単体テストをRSpecに変更
tags:
  - 'Rails'
  - Railsチュートリアル
private: false
updated_at: ''
id: null
organization_url_name: 'manabi-labo'
slide: false
ignorePublish: true
---

こんにちは！学びと成長しくみデザイン研究所の斉藤です。
本記事はRailsチュートリアルの第14章が終わった段階の[リポジトリ](https://github.com/yasslab/sample_apps/tree/main/7_0/ch14)をベースにして、より実践的なものへカスタマイズしていきます。

- カスタマイズ履歴
    - [letter_opener_web 導入](https://qiita.com/SaitoJP/items/f86085e6f5580b803a9f)
    - RSpec 導入 <= 今ココ

前回から引き続き、今回は RSpec を導入します。
Railsチュートリアルでは MiniTest を使用して単体テストを実装していますが、
多くの現場では RSpec で単体テストを実装しています。
よって、今回は MiniTest から RSpec へ切り替えを行います。



