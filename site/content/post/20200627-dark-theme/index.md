---
title: "ダークモードに対応させる"
date: "2020-06-27"
description: "gatsby-starter-blog で作ったブログをダークモードに対応させるためのメモ"
tags:
  - GatsbyJS
---

# はじめに

近頃は目に優しい（？）ダークモードな見た目が流行ってますよね。  
わたしも大好きで，設定できるものは片っ端からダークモードにしていますが，自分のブログが真っ白で目に悪い（？）なと思ったので，ダークモード化してみることにしました。

# TL; DR

* [insin/gatsby-plugin-dark-mode: A Gatsby plugin which handles some of the details of implementing a dark mode theme](https://github.com/insin/gatsby-plugin-dark-mode) の `README.md` の通りにする

# もう少しｋｗｓｋ

わかってる人は `TL; DR` の通りで一発だと思うのですが，Gatsby どころか JavaScript とか JS フレームワークとかなんじゃらほい，というわたしの場合はちょっと時間がかかりました。

## 詰まったポイント その1

しょっぱなのサンプルコードはどこに書いたらよいのやら？  
......としばらく悩んで，とりあえず `src/components/theme-toggler.js` に生やしておきました。

実際，これでいいのかどうかはよくわかりません。技術者としてあるまじき適当具合。

## 詰まったポイント その2

雛形に `gatsby-starter-blog` を使っている場合は `global.css` が存在しないので，自分で `global.css` を用意する必要があります。  
`components/styles/global.css` に書くことにしました。

## 詰まったポイント その3

plugin の README には `You can then use these variables in your site's components...` のあとにサンプルコードが書いてあります。  
なんとなく `Layout` クラスの実装サンプルのように見えるので，おそらく `components/layout.js` をいじるんだろうなと思いました。

が，いろいろ書いてあってどこをどうすればいいのやら。  
テンプレを理解せずに使いはじめて困り果てる，よい例ですね。

サンプルコードをしばらくにらめっこしながら，「これはおそらく `style` の中にキーワードを仕込んで CSS で定義した色変数を引っ張ってくればいいんだな」と気づいたので，そのようにしておきました。

もともとテンプレには `style` があって左右のマージンなどを調節しているようなので，そこにそっと差し込む感じです。  
わたしの例を貼っておきます。

```
diff --git a/src/components/layout.js b/src/components/layout.js
index 978c660..137a267 100644
--- a/src/components/layout.js
+++ b/src/components/layout.js
@@ -54,6 +54,9 @@ const Layout = ({ location, title, children }) => {
         marginRight: `auto`,
         maxWidth: rhythm(24),
         padding: `${rhythm(1.5)} ${rhythm(3 / 4)}`,
+        backgroundColor: `var(--bg)`,
+        color: `var(--textNormal)`,
+        transition: `color 0.2s ease-out, background 0.2s ease-out`,
       }}
     >
       <header>{header}</header>
```

## 詰まったポイント その4

例によってサンプルコードの `global.css` をそのまま写経して適用したところ，思ったような色になりませんでした。  
`darkslategray` という色が，暗めのグレーを想像していたところ，くすんだ緑っぽい色になって表示されたので，あれ？と。

色の問題なので，`$ gatsby develop` してローカルでサイトを表示させつつ，ブラウザのデバッガで色を変えてみて，しっくりくる感じの色合いにしました。  
今日（2020/06/27）現在の Firefox および Google Chrome 最新版で同じ色合いになることを確認できたので，ひとまずこれで運用することにします。

# おわりに

もう少し真面目に GatsbyJS のサイトを読んでフレームワークを理解する必要がありそうです。  
なんでも小手先ではうまくいかないもんですね。

# 参考リンク

* [insin/gatsby-plugin-dark-mode: A Gatsby plugin which handles some of the details of implementing a dark mode theme](https://github.com/insin/gatsby-plugin-dark-mode)

* [GatsbyにDarkModeをつける - Qiita](https://qiita.com/yoshiki-0428/items/aeb3ab1b58d41649fa52)

