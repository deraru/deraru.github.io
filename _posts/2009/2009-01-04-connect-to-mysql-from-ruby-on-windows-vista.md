---
layout: post
title:  Windows Vista上のRubyでMySQLに接続する
date:   2009-01-04 17:48:00 +09:00
categories:
    - tech
tags:
    - Ruby
    - MySQL
    - Windows Vista
---

Windows VistaでRubyからMySQLに接続できるようになった手順

# 環境

- OS：Windows Vista Ultimate SP1 x32
- One-Click Installer - Windows 1.8.6-27 Release Candidate 2インストール済み
- MySQL 5.1インストール済み

# 手順

1. 「スタート」→「すべてのプログラム」→「Ruby-XXX-XXX」→「RubyGems」→「RubyGems Package Manager」とクリックする
1. 「RubyGems Package Manager」画面で、「gem△install△dbi」と入力し「Enter」キーを押す（△は半角スペース）
1. 「gem△install△mysql△–no-rdoc」と入力し「Enter」キーを押す
1. 「gem△install△dbd-mysql」と入力し「Enter」キーを押す

# 参考資料

- [Ruby/DBIのインストール - tanamonの日記](http://d.hatena.ne.jp/tanamon/20081219/1229680986){:target="_blank"}
- ["mysql" 2.7.3 gem install for Windows appears incomplete - Ruby Forum](https://www.ruby-forum.com/topic/171779#759287){:target="_blank"}
