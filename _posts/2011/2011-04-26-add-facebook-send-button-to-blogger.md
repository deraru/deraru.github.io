---
layout: post
title:  Blogger に Facebook の Send Button （Sendボタン）をつける
date:   2011-04-26 19:23:00 +09:00
categories:
    - notes
tags:
    - XFBML
    - Facebook
---

The Facebook Blog の Sharing with Small Groups などで紹介された Send Button を Blogger につけたいと思います。とても簡単です。

以前にBlogger に「いいね！」ボタンをiframeでつけましたが、今回の Send Button はiframeではつけれないので、まとめてXFBMLでつけます。

1. Facebook JavaScript SDK を読み込んでいない場合はscriptタグを追加する
1. 既存の「いいね！」ボタンがある場合はタグを削除する
1. 「いいね！」と「Send」ボタンのタグを追加する

# Facebook JavaScript SDK を読み込んでいない場合はscriptタグを追加する

Blogger の管理画面で、「デザイン」タブから、「HTMLの編集」をクリックして、Facebook JavaScript SDK を読み込むようにします。 

# 既存の「いいね！」ボタンがある場合はタグを削除する

もしすでに「いいね！」ボタンをつけている場合は削除します。以前の記事ではiframeを使って「いいね！」ボタンをつけましたので、ここではそれを削除します。

# 「いいね！」と「Send」ボタンのタグを追加する

削除した部分に以下のコードを追加します。そのまま追加できると思います。

{% highlight html %}
<like expr:href="data:post.url" layout="button_count" send="true" show_faces="true" width="130"></like>
{% endhighlight %}

上記タグの中に、layoutオプションがありますので簡単に説明します。好みに合わせて変えてください。

- layout=
    - standard
        - 「いいね！」ボタンを押した人数が「Send」ボタンの横に長々と表示されます
    - button_count
        - 「いいね！」ボタンを押した人数がボタンの横に表示されます
    - box_count
        - 「いいね！」ボタンを押した人数が「いいね！」ボタンの上に表示されます

以上です。 以前紹介したように、きちんと Open Graph の設定をしていれば、「Send」ボタンを押したときに、記事のタイトルや画像などが表示されるはずです。Open Graph の設定もやっておきたいですね。 
