---
layout: post
title:  ActionController で空のレスポンスを返す
date:   2011-04-30 23:00:00 +09:00
categories:
    - tech
tags:
    - Ruby on Rails
    - ActionController
---

Ajax で処理のリクエストだけ投げて、処理が成功したか失敗したかだけ知りたいというようなことがありました。そのためだけに何も書いてないテンプレートを作るのはなんかいまいちだなーと思ってたら、テンプレートを用意せずに空のレスポンスを返すやり方がありました。

{% highlight ruby %}
render :nothing => true
{% endhighlight %}
