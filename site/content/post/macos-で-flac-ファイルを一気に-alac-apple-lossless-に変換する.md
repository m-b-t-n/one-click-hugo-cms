---
title: macOS で FLAC ファイルを一気に ALAC (Apple Lossless) に変換する
date: 2021-01-30T14:04:35.920Z
description: macOS mojave (v10.14.x) で FLAC ファイルを ALAC ファイルに一括変換する方法
---
# はじめに

コマンド一発で 1000+ の FLAC ファイルを一気に ALAC (Apple Lossless) に変換する方法のメモです。  
macOS で `ターミナル` （要は bash）を使ったことのある人，使い慣れている人向けです。  
なるべく平易に書き下すつもりですが，言葉足らずの部分があればご指摘頂ければ幸いです。

# TL; DR

* [XLD](https://tmkk.undo.jp/xld/) のコマンドラインツールを使います

* 適当な手段で `FLAC ファイルへのパス一覧.txt` を作ります

* `FILE="/path/to/file.flac"; xld -f alac -o "$(dirname "${FILE}")" "${FILE}"` で元ファイルと同じところに ALAC ファイルが生成されます

# 動作確認環境

* macOS mojave 10.14.6

* bash, version 3.2.57(1)-release (x86_64-apple-darwin18) on iTerm2 v3.3.7

# 手順

## その1: ツールの導入

* [XLD](https://tmkk.undo.jp/xld/) の dmg をダウンロードして，開きます

* `XLD.app` を `/Applications` にコピーします

* `CLI` フォルダの中にある `xld` を `/usr/local/bin` にコピーします

* `ターミナル` またはお使いのターミナルエミュレータを起動して， `which xld` と入力します

  - `/usr/local/bin/xld` と表示されれば OK です。次のステップに進みましょう

  - 何も表示されない等の場合は，どこかの手順を間違っている可能性があるので，戻って1つずつ確認しましょう

  - `which xld` のかわりに `xld --help` と入力して， `xld` のヘルプが表示されることの確認でも代用できます

## その2: FLAC ファイル一覧の作成

* どのような方法で一覧を作成してもよいです

  - この後のステップに必要なのは `カレントディレクトリから目的の FLAC ファイルまでの {相対, 絶対} パスの一覧` です

  - ここでは，拡張子 `.flac` を頼りに `find` で探し出すことにします

* `ターミナル` またはお使いのターミナルエミュレータを起動して，下記 cmd を入力します

```sh
$ find . -name "*.flac" > flac-files.txt
```

* `flac-files.txt` をお使いのテキストエディタまたは `less` 等で開き，目的の FLAC ファイルがリストアップされているかを確認します

## その3: お試し実行

* いきなり実行して失敗するのは怖いですよね？

* さきほど作成した一覧ファイルの一部を使って，期待通り FLAC to ALAC への変換ができるかチェックしておきます

* まずは一覧ファイルを適当に切り出します。`-n` のあとの数字はお好みでどうぞ

```sh
$ head -n10 flac-files.txt > test.txt
```

* 次に，下記の cmd を打ち込みます

  - 予め同様の script を書いて実行してもよいです。これぐらいならワンライナーでもなんとか

```sh
$ cat test.txt | while read FILE; do DIR="$(dirname "${FILE}")"; xld -f alac -o "${DIR}" "${FILE}"; done
```

* きっと "hoge.flac" と同列に "hoge.m4a" が出来上がっているはずなので，下記ポイントを中心に確認します

  - あらぬところに保存されていないか？

  - メタデータは保持されているか？

  - そもそも再生できるか（変換されたファイルが壊れていないか）？

## その4: 本番実行

* おまたせしました，あとはサマーウォーズばりに勢いよく cmd を打ち込んでよろしくおねがいしましょう

  - 私は心配性なので，あとで結果を確認できるように `tee` でログをとっておくことにしました

```sh
$ (cat flac-files.txt | while read FILE; do echo "Convert "${FILE}""; DIR=$(dirname "${FILE}"); xld -f alac -o "${DIR}" "${FILE}"; done) 2>&1 | tee convert.log
```

* ちなみに実行するとこんな感じのログが出ます

```sh
Convert ./林原めぐみ/01 集結の園へ.flac
|====================| 100% (Track 1/1)
done.
Convert ./林原めぐみ/02 集結の園へ ～AYANAMI Ver.～.flac
|====================| 100% (Track 1/1)
done.
Convert ./林原めぐみ/03 集結の園へ (off vocal version).flac
|====================| 100% (Track 1/1)
done.
Convert ./林原めぐみ/04 集結の園へ ～AYANAMI ver.～ (off vocal version).flac
|====================| 100% (Track 1/1)
done.
```

## その5: 重複ファイルの削除

* これからかんがえます！

（2019/01/11 修正）  
"その4" が確実に実行されている（変換失敗のない）ことが前提ですが  
"その2" で作成した FLAC ファイル一覧を使って下記のように実行することで，元の FLAC ファイルを削除することができます。  

```sh
$ cat flac-files.txt | while read FILE; do rm -fv "${FILE}"; done
```

# おわりに

いまどきの人の音楽を聞く手段はおおかた各種ストリーミングサービスなんでしょうね。  
この記事の賞味期限は果たしていつまでなのやら。

