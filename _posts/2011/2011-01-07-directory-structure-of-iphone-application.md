---
layout: post
title:  iPhone アプリでファイル保存に使うディレクトリ
date:   2011-01-07 21:10:00 +09:00
categories:
    - tech
tags:
    - iOS
    - iPhone
---

iPhone アプリでファイルの保存なんかをしたいときは、アプリケーションごとに用意されてるサンドボックス内のディレクトリを使う。
tmp と Library/Caches はその他のディレクトリと違って、ファイルの永続性・バックアップに特徴がある。 

- tmp
    - Mac の tmp ディレクトリと同じで、一時保存のために使う。
    - iPhone を再起動するとディスクから消される。
- Library/Caches
    - iPhone を再起動してもディスクから消されない。
    - iTunes でバックアップされない。
        - バックアップが早く終わる。
- 以外のディレクトリ（Documentsとか）
    - iTunes でバックアップされる。
        - バックアップに時間がかかるようになる。
