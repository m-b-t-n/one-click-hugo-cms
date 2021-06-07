---
title: "QNAP TS-431P で Time Machine を設定する"
date: "2021-06-07"
description: "QNAP TS-431P と Time Machine を使用して macOS のバックアップを継続的に作成する方法 "
tags:
  - macOS
  - Tips
---

# はじめに

macOS 標準のバックアップツールである Time Machine を使用して，NAS を宛先にバックアップを継続的に作成する方法をメモしておきます。

# 環境

## クライアント

1. Mac Mini Late 2018
  - OS: macOS mojave(10.14.6)
  - SSD: 512[GB]

2. MacBook Air M1 2020
  - OS: macOS Big Sur(11.0.x)
  - SSD 256[GB]

## サーバ

* NAS: [QNAP TS-431P](https://www.qnap.com/ja-jp/product/ts-431p)

* HDD: [WDC WD80EFAX](https://www.cfd.co.jp/product/hdd/3_5inch/wd80efax/) x2
  - RAID1 構成で運用
  - 種々のデータ保存用とは別個にストレージプールを作成

# 方法

## TL; DR

[QNAP 公式](https://www.qnap.com/ja-jp/how-to/tutorial/article/time-machine-%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%97%E3%81%A6-mac-%E3%82%92-smb-3-%E7%B5%8C%E7%94%B1%E3%81%A7-qnap-nas-%E3%81%AB%E3%83%90%E3%83%83%E3%82%AF%E3%82%A2%E3%83%83%E3%83%97%E3%81%99%E3%82%8B) が懇切丁寧に解説してくれているので，そちらを参照のこと。

## 変更点・注意点 など

### マシン−アカウントーバックアップフォルダ の手動紐付け

* マシンごとにユーザアカウントを新規に割り当て，クオータを 2048[GB] に設定した
* バックアップ用フォルダ（共有フォルダ）をマシンごとに作成し，アクセス権限を上記で作成したアカウントのみに設定した

### バックアップ用フォルダが Time Machine 上でマウントできない？

* 既に別のアカウントを使用して同名の NAS に接続している時に起こる？
  - データ保存用に運用していたアカウントでログインしたまま，設定しようとしていて，これに引っかかった

* 一度ログアウト（アンマウント）してから Time Machine 上でマウント以降の作業をすすめる
  - 我が家の環境では，これで解決

* 一度 Time Machine を設定してしまえば，その後は {データ保存用，バックアップ用} の複数アカウントで同時にログインして，それぞれの作業（データ管理，バックアップ，etc.）が可能★

# おわりに

実はもう半年ほどこの構成で運用してますが，特にトラブルもなく快適に利用できています。  
参考になれば幸いです。

