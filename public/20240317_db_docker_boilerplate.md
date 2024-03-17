---
title: ローカル開発用のデータベースをDockerで構成する際のおすすめボイラープレート
tags:
  - Docker
private: true
updated_at: '2024-03-17T18:12:30+09:00'
id: d0df0ff431ee184b895b
organization_url_name: null
slide: false
ignorePublish: false
---


こんにちは！学びと成長しくみデザイン研究所の斉藤です。
私はローカルの開発環境でDBを使用する際、よくDockerを使用しています。
Dockerを使用するとプロジェクト毎に異なる種類のDBやバージョンを簡単に切り替えることができて便利です。
今回はDocker-Composeの設定ファイルである `compose.yaml` のおすすめボイラープレートをご紹介します。

# おすすめのGitHubリポジトリ

https://github.com/luisaveiro/localhost-databases

- 特徴
    - RDB(MySQL, PostgreSQL, etc..)やNoSQL(redis, DynamoDB, etc..)など、様々な `compose.yaml` がある
    - `compose.yaml` のパスワードやポート番号などが `.env` で変数管理されている


## 試してみる

例としてmysqlのリポジトリを使ってみます。

1. mysqlのディレクトリを開く
    - https://github.com/luisaveiro/localhost-databases/tree/main/databases/mysql
1. `compose.yaml` と `.env.example` をローカルへコピーする
1. `.env.example` を `.env` へリネームし、`DB_ROOT_PASSWORD` を `pass` にする
1. 利便性向上のため `compose.yaml` 一部編集
    1. `services` に `adminer` を追加し、DBのブラウザから操作できるようにする

        ```
        adminer:
          image: adminer:latest
          container_name: adminer
          ports:
            - 8080:8080
          depends_on:
            - mysql
          networks:
            - local
        ```

    1. デフォルトの認証方式である `caching_sha2_password` だとTLSによる保護がされていない通信はエラーになり、他のホストのSQLクライアントから接続できないため、`mysql_native_password` に変更する

        ```
        # Use mysql_native_password instead of caching_sha2_password
        command: "--default-authentication-plugin=mysql_native_password"
        # ↑ コメントアウトされている箇所を有効にすればOK
        ```
1. ブラウザで `http://localhost:8080` にアクセスして以下のように入力するとDBへアクセスできる

    ※ `サーバ` の箇所は `compose.yaml` の `services` 名を入力します
    ![image001](https://raw.githubusercontent.com/SaitoJP/engineer_nootebook/main/images/20240317_001.png)


# さいごに

