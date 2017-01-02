---
layout: post
title:  Rails 5.1以降のControllerテストについて
date:   2016-12-31 17:46:02 +09:00
categories:
    - tech
tags:
    - Rails
    - Rspec
---

Rails 5.0でControllerのSpecを書いていたら、どうやら `assert_template`, `assigns` は本体から切り離されて、`rails-controller-testing` というGemに切り出されたらしいことを知った。

Controllerの単体テストとして、これまではコントローラー内の処理の分岐に応じて以下のSpecを必ず書いてきた。

- have_http_status を使ったレスポンスコードの確認
- render_template を使ったレンダーするViewの確認
- assigns を使ったレンダーするViewに渡すデータの確認

この内の2つが本体だけでは書けなくなった、というインパクトは大きかった。
じゃあControllerのテストでは何をテストするんだ、と疑問になったので、本体から切り離された背景を調べてみた。

まず、この2つのメソッドは別にRSpecの本体から切り離したわけではないということが分かった。Railsの本体から切り離されていた。
切り離しの議論が行われていたのはこのIssue [Deprecate assigns() and assert_template in controller testing · Issue #18950 · rails/rails](https://github.com/rails/rails/issues/18950){:target="_blank"}

すごくいい議論が行われているなーと思いながら、頑張って英語を全部読んだ。

ControllerとViewは切り離せるものではなく、ViewはいうなればControllerのプライベートメソッドみたいなものである。なぜなら、Controllerが返すのはコンパイルされたViewだからだ、という部分は腑に落ちた。
と同時に、RailsでControllerとViewの間で行われるデータの受け渡しのインターフェースが、インスタンス変数になっていることに、ControllerとViewを切り離せない原因の一端がありそうだなーと感じた。

そして将来的には、完全にControllerのテストは無くなるかもしれないと。

レンダーされるViewのテストをしたいならViewの単体テストをすればよく、リクエストからController内の処理の分岐に応じたViewのテストをしたいなら、Request（あるいはFeature）テストをすればいい。

Controllerから見たprivateメソッドたるViewへの受け渡しに使うインスタンス変数のアサインをテストするのは無駄なこと。が、もし本当にController内でアサインしたインスタンス変数のテストをしたい人がいるなら、本体から切り離したGemがあるからそれを使えば、一応できる。
ただし、それをテストするくらいなら、Request（Feature）テストを書くのがいいよねという理解に至った。

海外での事情は知らないが、独自の汚いコントローラが蔓延し、目的が曖昧になった巨大なControllerのテストが存在する実情を考えると、これは正しい方向かもしれないなと思った。
