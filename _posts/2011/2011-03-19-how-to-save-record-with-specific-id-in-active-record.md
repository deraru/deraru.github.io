---
layout: post
title:  Active Record で id を指定してデータを保存する方法
date:   2011-03-19 21:26:00 +09:00
categories:
    - tech
tags:
    - Active Record
    - Ruby on Rails
    - Facebook
---

Facebook のユーザーIDを、そのままRailsアプリのユーザーテーブルのIDとして使いたくて調べてみた。やり方は2つあった。

- find_or_create_by_idメソッドを使う
    - id以外の属性も指定して保存しようとすると2回DBに書き込みがかかる
- find_or_initialize_by_idメソッドを使う
    - 他の属性を同時に指定しても1回のDB書き込みで済む

参考URL

- [ruby on rails - Best way to find_or_create_by_id but update the attributes if the record is found - Stack Overflow](http://stackoverflow.com/questions/5160073/best-way-to-find-or-create-by-id-but-update-the-attributes-if-the-record-is-found){:target="_blank"}
