---
layout: post
title:  Ubuntu 上で Rails + heroku + SQLite or MongoDB + github 管理の環境を作る
date:   2011-04-30 06:25:00 +09:00
categories:
     - tech
tags:
    - heroku
    - github
    - Ruby on Rails
    - MongoDB
    - Mongoid
---

最初は Windows 7 上の RubyInstaller を使った環境でやってたんですが、どうにも遅かったので、今は VMware Player を入れてその上でタイトルのような環境を作って開発してます。仮想環境かつCPUもメモリも少ないのにこっちのほうが断然早い。というわけで構築メモです。

- VMware Player をインストール
- Ubuntu をインストール
- VMware Tools をインストール（勝手に出てくる）
- Google 日本語入力 をインストール

ここからはコマンドで。最小構成ではないし、ヒストリから抜き出したので違う部分もあるかも。

{% highlight shell %}
apt-get upgrade
apt-get install ruby rubygems apt-get install build-essential curl zlib1g-dev libssl-dev libreadline5-dev libxml2-dev libsqlite3-dev
apt-get install git
apt-get install sqlite3
apt-get install mongodb
bash
rvm install 1.9.2
rvm use 1.9.2
rvm gemset create rails3
rvm gemset use rails3
rvm use 1.9.2@rails3 --default
gem install rails --no-rdoc --no-ri
gem install sqlite3-ruby --no-rdoc --no-ri
gem install mongoid bson_ext --no-rdoc --no-ri
gem install heroku --no-rdoc --no-ri
gem install rspec-rails --no-rdoc --no-ri
gem install mongoid-rspec --no-rdoc --no-ri
gem install jquery-rails --no-rdoc --no-ri
cd .ssh/ ssh-keygen -t rsa -C "email@example.com"
cd ..
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
git config --global github.user yourusername
git config --global github.token YourToken
heroku keys:add ~/.ssh/id_rsa.pub
{% endhighlight %}

SQLite3 のアプリを作ってgithubとherokuにプッシュします。別途Gemfileの編集が必要です。

{% highlight shell %}
rails new YourAppName
cd YourAppName/
bundle install
rails g rspec:install
rails g jquery:install -ui
rails s
git init
git add .
git commit -m "Init"
heroku create YourAppName
heroku stack:migrate bamboo-mri-1.9.2
git remote add origin git@github.com:yourusername/appname.git
git remote add heroku git@heroku.com:YourAppName.git
git push origin master
git push heroku master
{% endhighlight %}

MongoDB のアプリの場合です。別途Gemfileとmongoid.ymlの編集が必要です。

{% highlight shell %}
rails new YourAppName -O
cd YourAppName/
bundle install
rails g rspec:install
rails g jquery:install -ui
rails g mongoid:config
rails s
git init
git add .
git commit -m "Init"
heroku create YourAppName
heroku stack:migrate bamboo-mri-1.9.2
heroku addons:add mongohq:free
git remote add origin git@github.com:yourusername/appname.git
git remote add heroku git@heroku.com:YourAppName.git
git push origin master
git push heroku master
{% endhighlight %}

参考資料

- http://help.github.com/win-set-up-git/
- http://help.github.com/create-a-repo/
- http://devcenter.heroku.com/articles/quickstart
- http://blog.madoro.org/mn/86
- http://thinkit.co.jp/story/2010/10/27/1829
