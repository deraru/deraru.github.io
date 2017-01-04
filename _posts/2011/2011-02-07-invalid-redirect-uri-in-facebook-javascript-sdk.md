---
layout: post
title:  Facebook JavaScript SDK のポート問題
date:   2011-02-07 11:32:00 +09:00
categories:
    - tech
tags:
    - Facebook
    - Google App Engine
    - JavaScript
---

Google App Engine 上で Facebook の JavaScript SDK を使ってWebアプリを作ってるわけですが、どうもURLにポート番号が含まれているとうまく動かない様子。
サンプルアプリでも問題が起こる。サンプルアプリはただ単に Facebook にログインして、そのユーザーの情報を取ってきてるだけ。Chrome 9 だとうまく動くけど、IE 9, Opera 11 だとダメで、

> API Error Code: 191API Error Description: The specified URL is not owned by the applicationError Message: redirect_uri is not owned by the application.

というエラーが表示される。
この時、Facebook のアプリページのサイトURLは「http://localhost:GAEのポート番号」と登録してあり、ブラウザからは「http://localhost:GAEのポート番号」に接続している。
これをサイトURL「http://localhost/」、ブラウザ「http://localhost:GAEのポート番号」に変えると上記のエラーは表示されなくなるけど、当然リダイレクト先のページにつながらないわけで、ログイン処理自体がうまくいかなくなる。
これは全くの推測なんだけど、IEとOperaでは JavaScript SDK がうまいことポート番号を拾えてないんじゃないかな？
とりあえず Chrome だけ使って開発してます。
