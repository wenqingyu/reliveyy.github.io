---
layout: post
title: Mac OS X Docker Quick Start Terminal 闪退问题
categories: bug
comments: true
tags: [docker, iterm2]
date: 2016-06-21 14:39
---

## 排查过程

打开 Console.app 发现每一次闪退的时候都会有如下几条日志出现

    launchservicesd[85]: SecTaskLoadEntitlements failed error=22
    launchservicesd[85]: SecTaskLoadEntitlements failed error=22
    appleeventsd[56]: SecTaskLoadEntitlements failed error=22
    launchservicesd[85]: SecTaskLoadEntitlements failed error=22
    tccd[86808]: SecTaskLoadEntitlements failed error=22

通过 Google 搜索发现这是[一个 iterm2 相关的问题](https://gitlab.com/gnachman/iterm2/issues/4447)，所以我猜测是最近升级了 iterm2 到 Build 3.0.2 导致了 Docker Quick Start Terminal 无法打开。

## 解决方法

修改 /Applications/Docker/Docker\ Quickstart\ Terminal.app/Contents/Resources/Scripts/main.scpt，注释掉下面这段代码，这样 Docker 就会启动系统默认的 Terminal.app。这个方法只能说暂时解决了问题，更好的解决方法还待进一步跟进。

    (* 
    try
        tell application "Finder" to get application file id "com.googlecode.iterm2"
        set itermExists to true
    end try
    *)

