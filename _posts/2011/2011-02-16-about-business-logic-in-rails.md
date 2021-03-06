---
layout: post
title:  Rails とデータベース、ビジネスロジックの関係
date:   2011-02-16 16:59:00 +09:00
categories:
    - tech
tags:
    - Active Record
    - Ruby on Rails
    - アーキテクチャ
    - RDB
---

Rails を使ってWebアプリを作るべく、少しいじってみたんだけど、どうもデータベースとビジネスロジックの扱いに気持ち悪さを感じる。感じるんだけどそれはそれでありなのかなとも思う。ちょっとよく分からない。Railsは初心者なので勘違いしてる部分もあるかも。

データベースの整合性に対して、DBMS（RDBを想定）とアプリ（Railsを想定）のどちらに責任を持たせた方がいいんだろうか。

論点は3つ。1つめは生産性（開発スピードとか）。2つめは保守性（開発後のメンテナンスとか）。3つめはデータの再利用性（他のアプリでそのデータベースを使えるかとか）。するとこんな感じになるかな。どちらも他方と比べての話。

所在|生産性|保守性|再利用性|その他
RDB|×<br />正規化、ストアドプロシージャ、ORマッピング、バージョン管理、全部大変|○<br />作るときに苦労する甲斐はある|○<br />直接使える|パフォーマンス良い（ストアドプロシージャ）<br />スケール大変
Rails|○<br />Active Record使い放題<br />Migrationし放題|？<br />やったことない|×<br />そのアプリを通してでないと使えない|パフォーマンス悪い<br />KVS使ってればスケール楽

うーん。明らかにどっちにもメリットある。将来性のあるDBMSか、アジャイルなRailsか。

個人的には、データは基本的にRDBに入れられてて、データの整合性はDBMSが責任を持って、DBMSとアプリ間のデータのやり取りはRESTかストアドプロシージャというアーキテクチャが好き。データベースはデータベースで独立したシステムというか、なんのアプリでも接続してこいや！てきな。

でもWebアプリをガンガン作っていきたい、ってときにはとりあえずRailsに乗っとけばいいのかな。万が一ヒットしてパフォーマンスの問題が出てきたら、それはその時に解決しましょうよ、というスタイル。

ビジネスロジック（トランザクション、あるいは複数テーブルの同時操作処理）はRDBが持ってた方がアーキテクチャ的にはきれいだとは思う。ただRailsの場合は、ビジネスロジックをアプリに持たせても、それに見合うだけの高生産性というメリットが十分にあるように思う。
