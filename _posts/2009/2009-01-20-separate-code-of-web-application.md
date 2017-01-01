---
layout: post
title:  Webアプリケーションのコード（サーバ）の分離
date:   2009-01-20 03:42:00 +09:00
categories:
    - tech
tags:
    - Web Application
    - アーキテクチャ
    - 考え
---

たらたら書いたけど、要するにデザインテンプレートが気に入らないってことらしい。

# はじめに

## 目的

Webアプリケーションを作るにあたって、作業の並行性とシステムのスケーラビリティの向上を実現するために、コードの分離と役割ごとのサーバの分離方法を考える。

# 何が気に入らないのか

## Webアプリケーションを構成する"もの"

それなりにWebアプリケーションを使ったことはあるけど、Webアプリケーションを作ったことは一度もない私が思うに、Webアプリケーションはざっとこのくらいの"もの"で構成されていると思われる

- HTML
- CSS
- JavaScript
- Media
    - 画像とか動画とか
- ApplicationLogic
    - JavaとかC#とかRubyとか
- DataBase
    - HTTP経由でデータ操作ができないデータソース
- DataCloud
    - HTTP経由でデータ操作ができるデータソース

何ていうか知らないものは適当に造語を当てはめた。

## Webアプリケーションに含まれる"要素"

ここであげるものたちのことをとりあえず"要素"と呼んでおく。

- 文章
    - 説明するための文だったり、指示するための文だったり、コンテンツそのものだったり
- 画像
    - コンテンツそのものだったり、文章で表現できないものを表現するものだったり
- 動画
    - コンテンツそのものだったり、文章で表現できないものを表現するものだったり
- 画面構成
    - ヘッダーはここにあって、ナビゲーションはここにあって、コンテンツ表示部分もあるとかを決めるもの
- 画面デザイン
    - ヘッダーはこうやって表示するとかを決めるもの
- クライアント側アプリケーション
    - クライアント側で起こったアクションを捕捉して別のアクションを起こすもの
- サーバ側アプリケーション
    - データ処理とかをするもの
- データ
    - アプリケーションに必要な情報

## "もの"と"要素"をマッピングする

"もの"と"要素"をマッピングすることで、どれが何を担ってるのかを見てみる。

もの|要素|補足
HTML|文章|タグで囲まれた部分
HTML|画面構成|タグ
CSS|画面構成|floatとか
CSS|画面デザイン
JavaScript|クライアント側アプリケーション|
Media|画像|
Media|動画|
ApplicationLogic|文章|HTMLを生成する場合
ApplicationLogic|画面構成|HTMLを生成する場合
ApplicationLogic|サーバ側アプリケーション|
DataBase|データ|アプリケーションに必要な内部からのデータ
DataCloud|データ|アプリケーションに必要な外部からのデータ

## HTMLとApplicationLogicがうまくない（´・ω・`）

"もの"同士の関連を見てみる。

- HTMLとCSS
    - HTML内にCSSへのリンク
    - HTMLのタグ・ID・クラスで関連付け
- HTMLとJavaScript
    - HTML内にJavaScriptへのリンク
    - HTMLのタグ・ID・クラスで関連付け
- HTMLとMedia
    - HTML内にMediaへのリンク
- HTMLとApplicationLogic
    - ApplicationLogicでHTMLを生成して、そのHTML内にApplicationLogicのコードを埋め込む
    - HTMLのIDで関連付けできるものもある
- ApplicationLogicとDataBase
    - TCP/IP接続
    - テーブルの主キーで関連付け

「ApplicationLogicでHTMLを生成して、そのHTML内にApplicationLogicのコードを埋め込む」
これがうまくないと思うし気に入らない。
一つのファイルに2種類のコードが含まれるから。
AIRとかSilverlightとか全く別物をクライアント側に用意すれば分離できるけど、そういったものは使いたくない場合どうすればいいんだろうか（´・ω・）

# JavaScriptとDataCloudに全部押し付けてみる

## ApplicationLogicとDataBaseをラップする

これまでApplicationLogicとDataBaseが担ってきたデータ処理・出力を、HTTPアクセスで間接的に処理・出力できるようにして、DataCloudにする。
DataCloudにしても外部に公開する必要はない。

## HTMLとApplicationLogicの間にJavaScriptをはさむ

データ処理をDataCloudに押し付けたので、残ってるのは処理結果の画面への反映だろうと思う。
これを、Jaxerを使ったサーバ側のJavaScriptに押し付ける。
サーバ側のJavaScriptからDataCloudにHTTPアクセスして、データ処理をDataCloudに行わせ、処理結果を取得後、サーバ側のJavaScriptで画面へ反映させる。

## するとこうなる

まずApplicationLogicとDataBaseがなくなる。なくなるというより、別のWebサービスとして分離される。
JavaScriptがクライアント側アプリケーションだけではなく、サーバ側アプリケーションも担うことになる。
ただし、複雑なデータ処理をJavaScriptがするわけではない。複雑なデータ処理はDataCloudにあるApplicationLogicがやってくれて、その処理結果を画面に反映させることを行う。
というわけでこんな構成になる。

![suruto](/images/2009/2009-01-20-suruto.jpg)

## もう一度"もの"と"要素"をマッピングする

するとこんな構成になる場合、どれが何を担っているかを見てみる。

もの|要素|補足
HTML|文章|変わりなし
HTML|画面構成|変わりなし
CSS|画面構成|変わりなし
CSS|画面デザイン|変わりなし
JavaScript-C|クライアント側アプリケーション|変わりなし
JavaScript-S|サーバ側アプリケーション|データ処理要求の中継処理結果の画面への反映
Media|画像|変わりなし
Media|動画|変わりなし
DataCloud|データ|内部・外部関係なくHTTP経由

押し付けた甲斐あって大分すっきりしたと思う。

# さらにDataCloudに押し付ける

HTML内の文章もデータとみなしていいんじゃないかと思う。もちろんそのままでもいいので、適所をDataCloudに押しつける。
文章を全部DataCloudに押し付けると、HTMLは単なる画面に表示する枠を定義するだけのものになる。
