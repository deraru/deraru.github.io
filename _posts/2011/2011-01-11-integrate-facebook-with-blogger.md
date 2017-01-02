---
layout: post
title:  Blogger を Facebook に対応させる方法
date:   2011-01-11 01:40:00 +09:00
categories:
    - notes
tags:
    - Open Graph
    - Facebook
---

Blogger を Facebook に対応させて、「いいね！」ボタンを配置する方法です。
ただ単に、 Social Plugin の Like Button を配置するだけでも動くけど、せっかく用意されてる Open Graph を使わないのはもったいない。
Open Graph を使うと、「いいね！」を押してくれたユーザーのウォールに表示されるフィードを修飾することができたり、アクセス解析出来たりと便利です。
まあ使わなくてもある程度は Facebook が勝手に修飾してくれるんだけどね。

メタタグについては、 Blogger に限らず他のブログ（はてなダイアリーとか Word Press とか）でも使えるんじゃないかな。書き方は変えないといけないだろうけど、書く内容は変わらないはず。

というわけでやり方。

- Blogger のHTMLを編集して Open Graph のメタタグを追加する
- Blogger のHTMLを編集して Like Button のタグを追加する

# Blogger のHTMLを編集して Open Graph のメタタグを追加する

Blogger の管理画面で、「デザイン」タブから「HTML の編集」をクリックして、名前空間の追加と、メタタグの追加をします。
まずは名前空間の追加から。htmlタグにxmlns属性追記します。こんな感じです。

{% highlight html %}
html b:version='2' class='v2' expr:dir='data:blog.languageDirection' xmlns='http://www.w3.org/1999/xhtml' xmlns:b='http://www.google.com/2005/gml/b' xmlns:data='http://www.google.com/2005/gml/data' xmlns:expr='http://www.google.com/2005/gml/expr' xmlns:fb='http://www.facebook.com/2008/fbml' xmlns:og='http://ogp.me/ns#'
{% endhighlight %}

次にメタタグの追加を。headタグ内にmetaタグを追記します。このブログでは以下の様にタグを追加しました。

{% highlight html %}
b:if cond='data:blog.pageType == "item"'
    meta content='article' property='og:type'/
    meta expr:content='data:blog.pageTitle' property='og:description'/
/b:if
b:if cond='data:blog.pageType == "archive"'
    meta content='article' property='og:type'/
    meta expr:content='data:blog.pageTitle' property='og:description'/
/b:if
b:if cond='data:blog.pageType == "index"'
    meta content='blog' property='og:type'/
/b:if
meta expr:content='data:blog.pageTitle' property='og:title'/
meta expr:content='data:blog.url' property='og:url'/
meta expr:content='data:blog.title' property='og:site_name'/
meta content='http://sphotos.ak.fbcdn.net/hphotos-ak-snc6/hs073.snc6/168316_158489634199008_151351411579497_304256_2897253_n.jpg' property='og:image'/
meta content='1568105426' property='fb:admins'/
meta content='151351411579497' property='fb:page_id'/
{% endhighlight %}

詳しくは http://ogp.me/ に書いてある通りですが、簡単に説明すると、

- メタタグの形式
    - meta content='プロパティ値' property='og:プロパティ名'
    - meta content='プロパティ値' property='fb:プロパティ名'
- b:ifタグはそのページが、ブログ固有のページか、アーカイブページか、ホーム画面かを判断しています
    - それぞれのページでメタタグを切り替えるため
- 使ったプロパティの説明
    - og:type
        - オブジェクト（そのページが紹介しているものという感じ）の種類を指定します。主な種類はこちら。指定するタイプによって必須のプロパティが出てきます。
    - og:description
        - オブジェクトの説明です。1, 2文程度で説明します。
    - og:title
        - オブジェクトのタイトルです。フィードに表示されます。
    - og:url
        - オブジェクトのURLです。グラフのIDとして使われ、このURLに他のプロパティは結び付きます。
    - og:site_name
        - サイト名・ブログ名
    - og:image
        - オブジェクトの画像です。
    - fb:admins
        - サイト管理者のFacebookアカウントのユーザーIDを指定します。複数人いる場合はカンマで区切って指定します。
    - fb:page_id
        - サイト・ブログのファンページがあれば、そのページIDを指定します。

設定し終わったら、 Facebook が用意しているURLリンターを使って、自分が意図したとおりにメタタグが読まれるか確認してみてください。
また、Facebook が用意しているインサイトを使って、アクセス解析ができるようになります。インサイトの右上にある「あなたのドメインのためのインサイト」から自身のブログのドメインを登録してください。

# Blogger のHTMLを編集して Like Button のタグを追加する

Blogger の管理画面で、「デザイン」タブから「HTML の編集」をクリックして、 Like Button のタグを追加をします。
僕が追加したのは以下のタグです。このまま追加できると思います。
このタグを、div class='post hentry'タグ内に追加します。（「ウィジェットのテンプレートを展開」にチェックを入れて、「post hentry」で検索すると見つかります。）
僕はそのタグの中にある、div class='post-footer-line post-footer-line-1'タグ内に追加しました。

{% highlight html %}
iframe allowTransparency='true' expr:src='"http://www.facebook.com/plugins/like.php?href=" + data:post.url + "&layout=button_count&show_faces=false&width=110&action=like&colorscheme=light&height=20"' frameborder='0' scrolling='no' style='border:none; overflow:hidden; width:110px; height:20px;'
{% endhighlight %}

以上です。
ちゃんと設定すれば使えるインサイトはかなり強力なんじゃないかな。みなさんも「いいね！」ボタン設置してみてください。
よかったら「いいね！」押してもらえるとうれしいです！
