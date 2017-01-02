---
layout: post
title:  外部サイト用の Facebook ファンページの作り方
date:   2011-01-20 19:23:00 +09:00
categories:
    - notes
tags:
- ファンページ
- Open Graph
- Facebook
---

※この記事の情報はすでに古くなっています。（2011/9/25追記）

前回は Blogger を Facebook に対応させて、「いいね！」ボタンの配置などをしました。
今回も引き続き Blogger を Facebook に対応させていきたいと思います。

今回のやり方は Blogger に限らず、 Facebook に対応した（ Open Graph 適用済みの）外部サイトなら使えます。（※外部サイト用の Facebook ファンページと書きましたが、通常の Facebook ファンページとは若干異なります。ただ、機能的には大差ありませんし、ゆくゆくは通常の Facebook ファンページと同じものになるのではないかと思います。）

というわけでやり方。

1. URLリンターで外部サイトのトップページをリントする
1. 自身で「いいね！」ボタンをクリックする
1. 「管理用ページ」を作成する
1. 外部サイト用の「いいね！」ボタンのコードを取得する

# URLリンターで外部サイトのトップページをリントする

前回使用した Facebook のURLリンターを今回も使います。
URLリンターで外部サイトのトップページ（このブログの場合は、 http://deraru.blogspot.com/ ）を入力して、Lintボタンをクリックします。

# 自身で「いいね！」ボタンをクリックする

Lint結果ページの下段に「いいね！」ボタンがありますので、さくっと自身で「いいね！」ボタンをクリックしてください。

![Like button link](/images/2011/2011-01-20-like-button-link.png)

# 「管理用ページ」を作成する

すると「管理用ページ」というリンクが表示されますので、「管理用ページ」リンクをクリックして外部サイト用の Facebook ファンページを作成します。

![Admin page](/images/2011/2011-01-20-admin-page.png)

# 外部サイト用の「いいね！」ボタンのコードを取得する

見た目は通常のファンページと同じですが、画面上段の黄色背景で書かれている通り、管理者しか見ることができません。以下に黄色背景内の分を引用しておきます。

> This is the administration interface for your webpage at http://deraru.blogspot.com/. You can see Insights and publish to the users that have liked your webpage. Only the administrators of the webpage can view this interface, other users are sent to the webpage.

ざっくり訳すと、このページは外部サイト用の管理画面です。インサイトを見たり、「いいね！」ボタンをクリックしたユーザーに対してウォールの投稿を流したりできます。管理者のみこの画面は見ることができ、それ以外のユーザーがこの画面にアクセスすると外部サイトに転送されます。って感じですか。

![Admin fan page](/images/2011/2011-01-20-admin-fan-page.png)

さて、外部サイト用の「いいね！」ボタンのコードを取得します。
まずこの管理用ページのURLをコピーします。
次に、４　ホームページで紹介の「「いいね！」ボックスを追加」ボタンをクリックします。
Like Box のページが表示されますので、「Facebook Page URL」に先ほどコピーした管理用ページのURLを貼り付け「Get Code」ボタンをクリックします。（Like Box ではなく、単なる「いいね！」ボタン用のコードが欲しい場合は、手順２の「いいね！」ボタンのすぐ下にコードがあります。）

以上です。
このブログでも右上にこのブログ用の「いいね！」ボタンを設置してますので、よろしければ Join us!
