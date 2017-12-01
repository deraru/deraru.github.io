---
layout: post
title:  SecureRandomをrefineしてtwittersafe_base64メソッドを追加した
date:   2017-12-03 00:00:00 +09:00
categories:
    - tech
tags:
    - Ruby
author: deraru
---

お手伝い先でURL safeなハッシュ値がついたURLをTwitterに投稿する、という機能がありました。

URL safeなのでなんの問題もなくTwitterに投稿できそうだな。と思っていたのですが問題がありました。

URLの末尾が hypen で終わると、 hypen の手前までをURLとして認識し、そのURLでリンクが作られるので誤ったリンクとなり、クリックすると 404 Not Found になってしまいました。

この問題を解消するために、 Ruby Kaigi 2017 で具体的な使用例を見た Ruby の Refinement を使って `SecureRandom` に `.twittersafe_base64` というメソッドを定義して、URL safeでかつTwitterにも投稿可能なハッシュ値を生成できるようにしました。

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

`SecureRandom.urlsafe_base64` の出力から hypen を 取り除き underscore に置き換えているだけです。

これを使ってハッシュ値を生成するように変更して、Twitterに誤ったリンクが投稿されることはなくなりました。

Refinement で手軽に Ruby の拡張が出来てすごく楽しかったし、実用的なことに使えたので満足でした。
