---
layout: post
title: MariaDB放了一段时间再打开就ERROR了
categories: Fuzzy
comments: true
tags: mariadb
---

今天照常执行`mysql.server start`，结果却是ERROR，打开错误日志发现

```
150612 01:38:58 mysqld_safe mysqld from pid file /usr/local/var/mysql/MacBookAir.pid ended
```

然后就拿着这条错误信息去询问不存在的网站，病急乱投医的我试了两个方法，一个是说删除掉`/usr/local/var/mysql`下面的ib_logfile0和ib_logfile1，我试了没有用。接着又去尝试在我的Home目录下重建数据库，也没有用。在这种黔驴技穷的情况下，我瞄了一眼`/usr/local/var/mysql`目录下面的内容，发现有两个文件`Air.local.err`和`Air.local.pid`，我才记起原来这段时间没碰Maria的过程中，我改了Hostname（从Air.local改成了MacBookAir），于是我删掉了这两个文件，之后Maria就愉快地运行起来了。



