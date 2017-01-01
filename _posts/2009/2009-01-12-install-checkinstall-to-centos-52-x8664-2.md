---
layout: post
title:  CentOS 5.2 x86_64にcheckinstallをインストールする（２）
date:   2009-01-12 22:34:00 +09:00
categories:
    - tech
tags:
    - CentOS
    - checkinstall
---

前回の記事でソースコードからRPMを作ってインストールということをやったけど、↓のRPMから直接インストールでもとりあえず動く。

{% highlight shell %}
cd /tmp
wget http://www.asic-linux.com.mx/~izto/checkinstall/files/rpm/checkinstall-1.6.1-1.i386.rpm
rpm -ivh checkinstall-1.6.1-1.i386.rpm
{% endhighlight %}

これでどこにどんなに問題があるか私には分からないので、詳しい人ご指摘ください（;´д`）

- 気になるところ
    - アーキテクチャが違う
    - CentOS用にパッケージ化されたものではない
