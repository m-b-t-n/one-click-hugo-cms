---
title: "macOS 版 OneDrive でキーチェーンアクセスが失敗したときの対処法"
date: "2020-05-24"
description: "macOS 版 OneDrive でキーチェーンアクセスが失敗したときの対処法"
---

# はじめに

最近 macOS mojave (10.14.6) で OneDrive にログインできなくて困っていました。  
**キーチェーンアクセスに失敗しました** と出るので，なんだろうと思って適当にググったら，MS 公式の Forum に解決策が載っていたので，書き記しておきます。

# 手順

* `Keychain Access.app` を起動する（Spotlight で `keychain` と打つとはやい）

* `OneDrive` で検索して出てきたキーチェーンを全て削除

* OneDrive を再起動し，ログインし直す

# 参考リンク

* [OneDrive Keychain doesn't exist? - Microsoft Community](https://answers.microsoft.com/en-us/msoffice/forum/msoffice_onedrivefb-mso_mac-mso_o365b/onedrive-keychain-doesnt-exist/5c259bf2-2abf-4b9f-9b44-4db97ddfbcd2)

