---
layout: post
title:  二回続けてShinjukurbに行ってきた
date:   2015-07-22 00:00:00 +09:00
categories:
    - tech
tags:
    - Ruby
    - 勉強会
---

# microservice化に向けて

- スペースマーケット
    - 二宮さん
- モノリシックなシステムをどうmicro serviceかしていくか
- 2014/04
    - Rails -> MySQL
- 2015/02
    - アプリ開発
        - rails -> MySQL, ElasticSerch
- 2015/06
    - アプリリリース
        - rails(web) -> MySQL, ElasticSerch
        - rails(app) -> MySQL, ElasticSerch
        - モデルはそれぞれにある！！１
            - Rails Engineを使ったらどうか
                - Rails Engineくらいgenerateしてみよう！
                - テスト書きにくい
                    - ダミーアプリが必要になる
                    - ほぼ専用なら親側に書いてしまう手もある
- 理想
    - rails(ストレージ不要) -> rails -> MySQL, ElasticSerch
- 検討
    - Garageみたいなものを独自実装
    - ActiveResource
        - 辛い理由は？
            - めちゃくちゃ使いづらい
                - RESTじゃない！
