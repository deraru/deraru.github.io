---
layout: post
title:  MongoDB (via Mongoid) で 1:N のテーブルを MapReduce !
date:   2011-04-27 21:30:00 +09:00
categories:
    - tech
tags:
    - MapReduce
    - Ruby on Rails
    - MongoDB
    - Mongoid
---

Rails で MongoDB を使ったアプリを作ってみてるわけですが、親子関係になってるテーブルでデータを集計したかったのでやってみました。

やりたかったことは、「プロジェクト」に「ユーザー」が「段階的」に「出資金」を出せるので、そのプロジェクト毎に出資金の合計値を出したい、とかそんな感じです。

テーブルはこんな感じ。

- User (id, name, ...)
- Project (id, name, ...)
- Rank (id, amount, ...)
- Investment (id, project_id, user_id, rank_id, ...)

この、「段階的」にを表すRankが曲者で、出資金額はここを見ないと取れないわけです。Investmentのrank_idをたどってRankを見ないと出資金額が分からない。が、 MapReduce で使う JavaScript で書いた関数からは自ドキュメントしか見えない！！１

仕方ないので、 JavaScript で Switch 文を使いました。まーRankが3ランクくらいしかないので力技で。

というわけでInvestmentの中身はこんな感じになりました。

{% highlight ruby %}
class Investment
  include Mongoid::Document
  include Mongoid::Timestamps
  belongs_to :user
  belongs_to :project
  belongs_to :rank

  def total_amount
    map = <<-MAP
      function() {
        var amount = 0;
        switch (this.rank_id.toString()) {
          case "#{Rank.where(:rank => 1).first.id}":
            amount = "#{Rank.where(:rank => 1).first.amount}";
            break;
          case "#{Rank.where(:rank => 2).first.id}":
            amount = "#{Rank.where(:rank => 2).first.amount}";
            break;
          case "#{Rank.where(:rank => 3).first.id}":
            amount = "#{Rank.where(:rank => 3).first.amount"};
            break;
        }
        emit(this.project_id, {amounts: amount});
      };
    MAP

    reduce = <<-REDUCE
      function(key, values) {
        var sum = 0;
        values.forEach(function(doc) {
          sum += doc.amounts;
        });
        return {amounts: sum};
      };
    REDUCE
    collection.map_reduce(map, reduce, :query => {:ranking_id => self.ranking.id}).find().first()["value"]["amounts"].to_i
  end
end
{% endhighlight %}

で、このtotal_amountをView/Controller側から使えば集計結果が取れます。こんな感じ。

{% highlight ruby %}
@project = Project.find(params[:id])@project.investments.first().total_amount
{% endhighlight %}

Switch 文でかなり無理やりですが、1:Nの親子関係になってるテーブルでも集計できました。もっと簡単な方法やいい方法があればぜひ教えてください。

たぶん正規化のし過ぎなんだろうなー。参照じゃなくて埋め込みにしちゃえばSwitch 文は使わなくてよくなりそう。

参考資料

- [Complex intersections, aggregation and grouping in Mongo - Google グループ](https://groups.google.com/forum/#!topic/mongodb-user/xIuDxHmad5o)
