---
layout: post
title: Mac OS X EI Capitan 下编译 OpenJDK 9
categories: java
comments: true
tags: [jdk9]
date: 2016-02-15 21:54
---

## 编译环境

* 操作系统版本：`10.11.3`
* Xcode 版本：`7.2.1`
* 已安装 XQuartz

OpenJDK 是使用 mercurial 来做项目版本控制的，命令是 `hg`，可以通过 Homebrew 来安装。


```sh
hg clone http://hg.openjdk.java.net/jdk9/jdk9 jdk9
cd jdk9
chmod u+x get_source.sh
./get_source.sh
chmod u+x configure
./configure --with-freetype-include=/opt/X11/include/freetype2 --with-freetype-lib=/opt/X11/lib --disable-warnings-as-errors
make all
```

编译好的内容在目录 `build/macosx-x86_64-normal-server-release/` 下面。

## 参考

1. [How to build Open JDK 9 on Mac OSX Yosemite](http://blog.shelan.org/2015/03/how-to-build-open-jdk-9-on-mac-osx.html)
2. [Build errors on OS X "Failed module access verification"](http://permalink.gmane.org/gmane.comp.java.openjdk.build.devel/16341)
3. [OSX 10.11 El Capitan下编译OpenJDK 9](http://fpgatalk.com/osx-10-11-el-capitan%E4%B8%8B%E7%BC%96%E8%AF%91openjdk-9/)
