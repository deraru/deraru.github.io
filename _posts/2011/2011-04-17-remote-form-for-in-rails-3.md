---
layout: post
title:  Rails 3 で remote_form_for みたいなことをやる
date:   2011-04-17 22:41:00 +09:00
categories:
    - tech
tags:
    - Ruby on Rails
---

オライリーの Head First Rails を読みながら Rails の勉強してるわけですが、やってる環境が Rails 3 なんでちょっと違ったりする部分があったりします。（この本は Rails 2.3 に対応とのこと）

まあそんなに違わなかったので結構サクサク進んでたのですが、今回はちょっとつまったのでメモ書きしておきます。

Rails 3 らしいやりかたということではないと思います。あくまで本を進めるために、 2.3 のやり方をできるだけそのまま 3 でやるにはどうしたらいいんだろう、ということでやってます。

つまったのはP.287の `remote_form_for` です。このタグを使えば簡単にAjax処理が組み込まれるとのことですが、Rails 3 ではそもそも `remote_form_for` タグが無いようで動きません。どうするか。

まず、jquery-railsをインストールします。

次にAjax処理させたいフォームのタグを

{% highlight ruby %}
<%= form_for(seat, :remote => true, :html => {"data-update" => "seats", "data-type" => "html"}) do |f| %>
{% endhighlight %}

と変更します。

最後に、JavaScriptを書きます。僕はshow.html.erbにそのまま書きました。

{% highlight javascript %}
$(function() {
    $("form[data-update]").live("ajax:success", function(data,status,xhr) {
        var link = $(this);
        $("#" + link.attr("data-update")).html(status);
    });
});
{% endhighlight %}

これでちゃんと本の通り座席部分が非同期で更新されるはずです。
