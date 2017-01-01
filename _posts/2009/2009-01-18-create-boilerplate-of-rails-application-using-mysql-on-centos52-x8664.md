---
layout: post
title:  CentOS5.2 x86_64でMySQLを使うRuby on Railsのひな型を作る
date:   2009-01-18 18:11:00 +09:00
categories:
    - tech
tags:
    - Ruby on Rails
    - CentOS
    - MySQL
---

# 環境

- OS：CentOS 5.2 x86_64
- mysql-serverインストール済み

# 手順

1. mysql-develをインストール
1. gccをインストール
1. rubyをインストール
1. rubygemsをインストール
1. ruby-develをインストール
1. railsをインストール
1. mysqlアダプタをインストール
1. ひな型を作成
1. 確認

## mysql-develをインストール

1. yum install mysql-devel

## gccをインストール

1. yum install gcc

## rubyをインストール

1. yum install ruby

## rubygemsをインストール

1. yum install rubygems

## ruby-develをインストール

1. yum install ruby-devel

## railsをインストール

1. gem install rails

## mysqlアダプタをインストール

1. gem install mysql -- --with-mysql-dir=/usr --with-mysql-lib=/usr/lib64/mysql

## ひな型を作成

1. mysqlにログインする
1. create database demo_development;
1. mysqlからログアウト
1. rails -d mysql demo
1. cd demo
1. vi config/database.ymlで以下の行を修正

{% highlight yaml %}
development:
    password: 「username:に指定しているユーザのパスワード」
{% endhighlight %}

## 確認

1. script/server
1. ブラウザで「http://127.0.0.1:3000」にアクセスする
1. 「About your application’s environment」をクリックして「Database adapter」が「mysql」になっていることを確認する

# 参考にした主なサイト

- [Iwazer Report: CentOS4.4 x86_64にRails](http://www.iwazer.com/~iwazawa/diary/archives/003066.html)
- [Ruby on RailsのRJSでかんたんAjax開発（前編）：かんたんAjax開発をするためのRuby on Railsの基礎知識 (1/4) - ＠IT](http://www.atmarkit.co.jp/ait/articles/0808/25/news123.html)
