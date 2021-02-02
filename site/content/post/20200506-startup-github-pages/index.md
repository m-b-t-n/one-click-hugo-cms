---
title: "GitHub Pages リポジトリを立ち上げる"
date: "2020-05-06"
description: "GitHub Pages でブログを書くための下準備"
tags:
  - GitHub
  - GitHub Pages
---

# はじめに

今まで はてなブログさんにお世話になっていたのですが

* [GitHub Pages](https://pages.github.com/) が気になった

* そろそろ Web 周りの知識をつけたくなってきた

ということで，作ってみることにしました。

## TL; DR

1. [公式ページ](https://pages.github.com/) に従ってリポジトリを作成する

2. `index.html` を用意して `origin/master` に push

3. ブラウザで `https://username.github.io/` を開いて `index.html` の内容が反映されているか確認する

## もう少し細かい手順

`1.` の部分は省きます

### リポジトリを持ってくる

```
$ cd workspace
$ git clone git@github.com:m-b-t-n/m-b-t-n.github.io gh-pages
Cloning into 'gh-pages'...
warning: You appear to have cloned an empty repository.

$ cd gh-pages
$ pwd
$ ~/workspace/gh-pages
```

### 何はともあれ，まずは空コミットする

```
$ git commit --allow-empty -m "Initial Commit"
[master (root-commit) f9af6e7] Initial Commit
```

### "Hello, world" 的なものを用意する

```
$ echo "Under construction." >> index.html
$ git status
On branch master
Your branch is based on 'origin/master', but the upstream is gone.

 (use "git branch --unset-upstream" to fixup)

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	index.html

nothing added to commit but untracked files present (use "git add" to track)
```

### `index.html` をうｐ

```
$ git add index.html
$ git commit -m "Add index.html"
[master 259b13e] Add index.html
 1 file changed, 1 insertion(+)
 create mode 100644 index.html

$ git push -u origin master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (5/5), 402 bytes | 402.00 KiB/s, done.
Total 5 (delta 0), reused 0 (delta 0)
To github.com:m-b-t-n/m-b-t-n.github.io
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

### アクセスしてチェック

* ブラウザで `https://<username>.github.io/` を開いて `index.html` の内容が反映されているか確認する

  * `<username>` の部分は，適宜自分の GitHub アカウント名に置き換えること

# まとめ

* [GitHub Pages](https://pages.github.com/) で自分のブログを立ち上げるための下準備ができた

* ここまでは超お手軽，ここからが大変そう

* 次回につづく！

