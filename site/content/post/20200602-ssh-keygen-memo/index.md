---
title: "私的 SSH 鍵認証メモ"
date: "2020-06-02"
description: "ssh-keygen で SSH 鍵ペアを新規作成して使えるようにするまでのメモ"
---

# はじめに

時々 `ssh-keygen` で SSH 鍵ペアを作って認証に利用することがあります。ありますよね？  
他の人がどうだかは知らないですが，私個人としては利用する {サービス, マシン, etc.} 毎に鍵ペアは変えたいので，それなりの頻度で使います。

で，「鍵長どうすんだっけ？」とか「ファイル指定どうすんだっけ？」とか，よく忘れるので，それのメモです。

# TL; DR

* [SSH キーを生成して ssh-agent に追加する - GitHub ヘルプ](https://help.github.com/ja/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) をベースに `-f` オプションで鍵名を指定する

* [SSH 接続をテストする - GitHub ヘルプ](https://help.github.com/ja/github/authenticating-to-github/testing-your-ssh-connection) で確認する

# 書き下した手順

以下，[GitHub](https://github.com) に鍵を登録することを想定した手順です。  
が，例えば ConoHa VPS などの他サービスでも概ね同じやり方で認証できるようになると思います。

## 1. 鍵ペアの作成

`ssh-keygen` で SSH 鍵ペアを作ります。

```sh
### Generate "id_rsa_hoge" and "id_rsa_hoge.pub"
$ MAIL_ADDR="hoge@fuga.com"
$ KEY_NAME="id_rsa_hoge"
$ ssh-keygen -b 4096 -t rsa -C "${MAIL_ADDR}" -f ~/.ssh/${id_rsa_hoge}

### Paste public key to system clipboard (for macOS)
### If you use Linux, substitute "xsel" command.
### Or if you use Windows(Git Bash), substitute "clip" command.
$ cat ~/.ssh/${id_rsa_hoge}.pub | pbcopy
```

## 2. ssh config の設定

好みのエディタ `${EDITOR}` で `~/.ssh/config` を編集します。  

```sh
$ ${EDITOR} ~/.ssh/config
```

macOS の場合，下記を書き込みます。

```sh
Host github.com
	AddKeysToAgent yes
	UseKeyChain yes
	IdentityFile ~/.ssh/${id_rsa_hoge}
```

## 3. ssh-agent の設定

\# この辺はあまり理解してないので後々調べて理解しておく..

```sh
### Wakeup ssh-agent
$ eval "$(ssh-agent -s)"

### Register the key to "ssh-agent" (for macOS)
$ ssh-add -K ~/.ssh/${id_rsa_hoge}
```

## 4. 接続テスト

`Permission denied` となる場合は何かの設定が足りてないと思われるので1つずつ確認します。  
原因を掴むために `-v` オプションを `ssh` コマンドに付与するのもアリ。

```sh
$ ssh -vT git@github.com
Hi ${your_username_of_github}! You've successfully authenticated, but GitHub does not provide shell access.
```

# 参考リンク

* [SSH キーを生成して ssh-agent に追加する - GitHub ヘルプ](https://help.github.com/ja/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

* [SSH 接続をテストする - GitHub ヘルプ](https://help.github.com/ja/github/authenticating-to-github/testing-your-ssh-connection)

