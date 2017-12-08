---
layout: post
title:  SecureRandom を refine して twittersafe_base64 メソッドを追加した
date:   2017-12-03 00:00:00 +09:00
categories:
    - tech
tags:
    - Ruby
author: deraru
---

お手伝い先で URL safe なハッシュ値がついた URL を Twitter に投稿する、という機能がありました。

URL safe なのでなんの問題もなく Twitter に投稿できそうだな。と思っていたのですが問題がありました。

URL の末尾が hyphen で終わると、 hyphen の手前までを URL として認識し、その URL でリンクが作られるので誤ったリンクとなり、クリックすると 404 Not Found になってしまいました。

この問題を解消するために、 Ruby Kaigi 2017 で具体的な使用例を見た Ruby の Refinement を使って `SecureRandom` に `.twittersafe_base64` というメソッドを定義して、 URL safe でかつ Twitter にも投稿可能なハッシュ値を生成できるようにしました。

具体的なコードはこちら。

```ruby
require 'securerandom'

module SecureRandom
  module TwittersafeBase64
    refine SecureRandom.singleton_class do
      def twittersafe_base64(n=nil, padding=false)
        s = urlsafe_base64(n, padding)
        s.tr!("-", "_")
        s
      end
    end
  end
end
```

`SecureRandom.urlsafe_base64` の出力から hyhpen を 取り除き underscore に置き換えているだけです。

これを使ってハッシュ値を生成するように変更して、 Twitter に誤ったリンクが投稿されることはなくなりました。

Refinement で手軽に Ruby の拡張が出来てすごく楽しかったし、実用的なことに使えたので満足でした。
