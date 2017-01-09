---
layout: post
title:  MySQL User Conference Tokyo 2015に行ってきた
date:   2015-12-15 00:00:00 +09:00
categories:
    - tech
tags:
    - MySQL
    - 勉強会
---

# チューニング用メモ

- select_type
    - DERIVED
        - tempテーブル作ってる時
- filtered
    - 絞り込まれる見込みの比率
    - rows * filteredが見積もり行数
- optimizer_trace
    - rows
    - cost
- オプティマイザヒント
- パーティションテーブル
    - 日時で分けて、データの一括削除
