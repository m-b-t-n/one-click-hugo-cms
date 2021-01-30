---
title: 'BSD 系 xargs で "xargs: unterminated quote" が出る時の対処法'
date: 2021-01-30T14:08:41.678Z
description: " "
---
# はじめに

あけおめことよろなんとやら。  
最近，そこまで Linux 関連の仕事でご飯を食べているわけでもないので，ブログのタイトルを変えました。  
あといくつかネタ溜まってきたので，久々に真面目に書きます。

# 動作確認環境

* macOS mojave 10.14.6

* bash, version 3.2.57(1)-release (x86_64-apple-darwin18) on iTerm2 v3.3.7

# 本題

`find` した結果を `xargs` でバッサバッサ処理したい時に出てくるやつ。  
[こちら](https://www.shigemk2.com/entry/2019/04/01/185315) のページを参考にして解決。

ポイントは

* `find` する際に `-print0` オプションをつける

* `xargs` で `find` の結果を受けるとき，区切り文字を `-0` で null 文字 `\0` に変更する

の2つ。

下記，参考例； "._*.mp3" のようなゴミファイルを探し出して一覧表示するとき

```sh
$ find . -name "\._*\.mp3" -print0 | xargs -0 basename
```

いらないね，消したいね，って思ったら， `rm`  に渡してやればおｋ

```sh
$ find . -name "\._*\.mp3" -print0 | xargs -0 -I% rm -f "%"
```

\# `-I%` して確実に `rm -f` に渡す  
\# ダブルコーテーションで囲んで変な path を掴まないように配慮する

# おわりに

GNU/Linux のバイナリ入れた方が楽だったかしら。変わらないか。

本年もどうぞよろしくお願いいたします。
