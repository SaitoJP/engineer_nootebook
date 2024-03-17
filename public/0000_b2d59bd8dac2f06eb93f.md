---
title: Terminalの背景色を ssh 接続先に合わせて変更する自作コマンド
tags:
  - Mac
  - iTerm2
private: false
updated_at: '2024-03-17T11:00:45+09:00'
id: b2d59bd8dac2f06eb93f
organization_url_name: manabi-labo
slide: false
ignorePublish: false
---
# 概要

本番環境に ssh 接続した後、ローカル環境と間違えて誤ったコマンドを打たないように Terminal の背景色を動的に変更する

# 設定

１． `~/bin` ディレクトリを作成する
２． `~/.zprofile` に `export PATH="$HOME/bin:$PATH"` を追加する
３． `~/bin/sshh` ファイルを作成し以下記載

```sh:sshh
#!/bin/bash

set_term_bgcolor(){
  local R=$1
  local G=$2
  local B=$3
  /usr/bin/osascript <<EOF
tell application "iTerm"
  tell the current window
    tell the current session
      set background color to {$(($R*65535/255)), $(($G*65535/255)), $(($B*65535/255))}
    end tell
  end tell
end tell
EOF
}

function tab-reset() {
   NAME="Default"
   echo -e "\033]50;SetProfile=$NAME\a"
}

trap "tab-reset" INT EXIT # exitで背景色を戻す
if [[ "$2" == prd ]]; then
  set_term_bgcolor 100 0 0
elif [[ "$2" == stg ]]; then
  set_term_bgcolor 0 0 100
else
  set_term_bgcolor 0 100 0
fi

ssh $1
```
４． `$ chmod 755 ~/bin/sshh` で実行権限を付与

# 使い方

`$ sshh [sshの引数] [prd または stg または 引数なし]`

```sh
# production 環境に接続（背景色が赤）
$ sshh xxxx@xxxx prd

# staging 環境に接続（背景色が青）
$ sshh xxxx@xxxx stg

# その他（背景色が緑）
$ sshh xxxx@xxxx
```
