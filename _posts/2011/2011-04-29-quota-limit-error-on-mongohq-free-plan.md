---
layout: post
title:  MongoHQ Free で MapReduce を使っているとエラーが起こることがある
date:   2011-04-29 10:46:00 +09:00
categories:
    - tech
tags:
    - MapReduce
    - MongoDB
    - MongoHQ
---

heroku + MongoHQ Free の環境で Collection の追加がきなくなってしまい、しかも Collection の中身を見ようとすると、 Oops message が表示されてしまうので Collection の削除もできないという状況になりました。

そこでMongoHQ のサポートエンジニアの方に助けを求めると、素早く復旧してくれました。 その後お話を聞いているとこんな話題が。

> これはFree plan のQuota limit を越えたときに起きる事象だ。
> Free plan でも MapReduce はできる。でも時々 Free plan での MapReduce はおかしな挙動をすることがあるんだ。君の場合もまだ limit に達していないしね。今改善に向けて取り組んでるところだよ。

なるほどねー。Free plan じゃなければ起こらないのか。
