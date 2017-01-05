---
layout: post
title:  Blogger に Facebook Comments を設置する
date:   2011-04-26 21:45:00 +09:00
categories:
    - notes
tags:
    - XFBML
    - Facebook
---

Send Button をつけたので、ついでに Facebook Comments も設置しました。

Blogger 標準のコメント機能をなくしたので大分すっきり。すでにコメントがいっぱいついてるブログではどうするか迷うところでしょうね。

というわけでやり方。

- Blogger のコメント機能を無効にする
- Facebook Comments 用のタグを追加する

# Blogger のコメント機能を無効にする

ここに書くまでもないことですが一応書いておきます。

Blogger の管理画面で、「設定」タブから「コメント」をクリックします。

表示されたページで「コメント」を「表示しない」に、「コメントのデフォルトの設定」を「新しい投稿でコメントを禁止」に設定して、画面最下段の「設定を保存」をクリックします。

# Facebook Comments 用のタグを追加する

次に、「デザイン」タブから、「HTMLの編集」をクリックして、Facebook Comments 用のタグを追加します。

まず、Open Graph のタグをタグ内に追加します。以前に Open Graph のタグを追加済みであればやる必要はありません。

追加するタグは、以下の通りです。このタグはそのまま使えません。contentの数字を、自身の Facebook id に変えてください。詳しくは以前の記事を読んでください。

{% highlight html %}
<meta content="1568105426" property="fb:admins">
{% endhighlight %}

続いて Facebook Comments 用のタグを追加します。

追加する場所は、 `<div class="post-footer-line post-footer-line-3">` 内にしました。もし、 `<div class="post-footer-line post-footer-line-3" />` とタグの最後にスラッシュが入っている場合は、 `<div class="post-footer-line post-footer-line-3"></div>` と書き換えてください。追加するタグは以下の通りです。このまま追加できると思います。

{% highlight html %}
<comments expr:href="data:post.url" num_posts="2" width="500"></comments>
{% endhighlight %}

追加した結果僕の場合はこういう風になりました。

{% highlight html %}
<div class="post-footer-line post-footer-line-3">
    <comments expr:href="data:post.url" num_posts="2" width="500"></comments>
</div>
{% endhighlight %}

以上です。
