# はじめに

Qiitaの記事を管理するリポジトリです。

# よく使うコマンド

1. ログイン
    1. npx qiita login
2. 記事のプレビュー
    1. npx qiita preview
3. 新しい記事を追加
    1. npx qiita new (記事のファイルのベース名)
4. 記事を投稿、更新する
    1. npx qiita publish (記事のファイルのベース名)
    2. npx qiita publish --all
5. コマンドのヘルプ
    1. npx qiita help

# 画像を添付する

1. `images` ディレクトリ配下に画像を配置(ファイル名は先頭に `yyyymmdd_` を付与)
1. 以下の通り、ファイル名の先頭にgithubのパスを付与する
    ```
    ![image001](https://raw.githubusercontent.com/SaitoJP/engineer_nootebook/main/images/20240317_001.png)
    ```
