---
layout: post
title:  Facebook の認可・認証システムのセキュリティ問題
date:   2011-03-13 02:09:00 +09:00
categories:
    - tech
tags:
    - FBML
    - Facebook
    - JavaScript
    - Security
---

※2011/3/20追記　FB.uiを使っても、Cookieにaccess_tokenが書かれてました。となるとFacebookのSDKがそこら辺のことをやってそうな気がします。これ大丈夫なのかなー？SSL必須にするしかないのかな？

※2011/3/22追記　FB.initのcookieをfalseにすればCookie作られない！これでよし！

Facebook の認可・認証システムを利用する手段はいろいろある。例をあげるとこんな感じ。

- クライアント（ブラウザ）完結型
    - JavaScript SDK の FB.login を使う
    - FBML の fb:login-button タグを使う
- クライアント＋サーバー型
    - JavaScript SDK の FB.ui を使う
    - Oauth の手順通りコードを書く

で、これダメなんじゃないかって思うのが、クライアント完結型のやつ。

クライアント完結型のやつでは Facebook アカウントでログインした後、 Cookie に access_token がまるっとそのまま書かれてる。

HTTP では Cookie 丸見えだから、誰でもその人の access_token 見れちゃう。見れちゃうってことは盗まれちゃう。盗まれちゃったら、その access_token で許可されてる範囲で何でもされちゃう。

クライアント完結型のやつは、常時強制 SSL にして使うか、盗まれても影響が少ないように基本情報だけを要求するようなアプリ（要するに認証しか必要ないアプリ）でだけ使うのかな。 常時強制 SSL + FB.login が簡単でいいなー
