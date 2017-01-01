---
layout: post
title:  CentOS 5.2 x86_64にcheckinstallをインストールする
date:   2009-01-12 13:32:00 +09:00
categories:
    - tech
tags:
    - CentOS
    - checkinstall
---

CentOS 5.2 x86_64にyumでcheckinstallをインストールできなかったので自力でインストールしてみました。
警告いっぱい出ます。
とりあえず使えるっぽいからいっかｗ

# 環境

- OS：CentOS 5.2 x86_64

# 手順

1. gccをインストール
1. rpm-buildをインストール
1. ソースコードからcheckinstallをインストール
1. RPMからcheckinstallを再インストール

## gccをインストール

1. yum install gcc

## rpm-buildをインストール

1. yum install rpm-build

## ソースコードからcheckinstallをインストール

1. cd /tmp
1. wget http://www.asic-linux.com.mx/~izto/checkinstall/files/source/checkinstall-1.6.1.tgz
1. tar zxf checkinstall-1.6.1.tgz
1. cd checkinstall-1.6.1/installwatch-0.7.0beta5
1. vi installwatch.c.patch
1. 以下のパッチをコピペして保存
1. patch
1. cd ../
1. makemake install

{% highlight c %}
--- installwatch.c.ORG 2007-04-07 14:27:23.000000000 -0400
+++ installwatch.c 2007-04-07 14:25:06.000000000 -0400
@@ -84,7 +84,7 @@
static int (*true_open)(const char *, int, …);
static DIR *(*true_opendir)(const char *);
static struct dirent *(*true_readdir)(DIR *dir);
-static int (*true_readlink)(const char*,char *,size_t);
+static ssize_t (*true_readlink)(const char*,char *,size_t);
static char *(*true_realpath)(const char *,char *);
static int (*true_rename)(const char *, const char *);
static int (*true_rmdir)(const char *);
@@ -546,7 +546,7 @@
struct utimbuf timbuf;
size_t truesz;
char linkpath[PATH_MAX+1];
- size_t linksz;
+ ssize_t linksz;

#if DEBUG
debug(2,"copy_path(%s,%s)\n",truepath,translroot);
@@ -1582,7 +1582,7 @@
struct stat reslvinfo;
instw_t iw;
char wpath[PATH_MAX+1];
- size_t wsz=0;
+ ssize_t wsz=0;
char linkpath[PATH_MAX+1];

@@ -2698,8 +2698,8 @@
return result;
}

-int readlink(const char *path,char *buf,size_t bufsiz) {
- int result;
+ssize_t readlink(const char *path,char *buf,size_t bufsiz) {
+ ssize_t result;
instw_t instw;
int status;
{% endhighlight %}

## RPMからcheckinstallを再インストール

1. /usr/local/sbin/checkinstall
1. 「Please choose the packageing～」と表示されたら「R」を入力し「Enter」キーを押す
1. rpm -Uvh –nomd5 /usr/src/redhat/RPMS/x86_64/checkinstall-1.6.1-1.x86_64.rpm -excludepath /selinux

# 参考にした主なサイト

- [Freak: RHEL5 x86_64にcheckinstall-1.6.1をインストールしようとしたらmakeでエラー](http://ysmt.blog21.fc2.com/blog-entry-232.html){:target="_blank"}
- [Re: Cannot compile checkinstall on x86_64](http://checkinstall.izto.org/cklist/msg00256.html){:target="_blank"}
- [Freak: checkinstallのインストール](http://ysmt.blog21.fc2.com/blog-entry-168.html){:target="_blank"}
- [using checkinstall](http://www.patrickmin.com/linux/tip.php?name=checkinstall){:target="_blank"}
