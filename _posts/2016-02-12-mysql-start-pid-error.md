---
layout: post
title: MySQL 启动错误：The server quit without updating PID file
categories: tricks
comments: true
tags: mysql
---

我是在 Mac 上用 Homebrew 安装的 MySQL，不知道什么原因，启动 MySQL 的时候突然就产生了如下的错误提示：

```
ERROR! The server quit without updating PID file
```

我尝试了很多方法，最终起作用的方式是：

* 删除 `/usr/local/var/mysql` 目录下面所有的 `.err` 文件
* 将 `/var/lib/mysql` 目录的权限改为 777（只是图省事，这么做的原因是错误日志上面有一条错误提示是，无法在该目录内创建 mysql.sock.lock 文件） 
* `ln -s /var/lib/mysql/mysql.sock /tmp/mysql.sock`
* 执行 mysql_upgrade

### 参考
1. [MySQL服务器启动错误 'The server quit without updating PID file'](http://pein0119.github.io/2015/03/25/MySQL%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%90%AF%E5%8A%A8%E9%94%99%E8%AF%AF-The-server-quit-without-updating-PID-file/)
