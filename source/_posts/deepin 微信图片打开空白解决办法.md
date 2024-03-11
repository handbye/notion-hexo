---
permalink: 2019/11/29/deepin-wechat-img-issue
tags:
  - deepin
layout: post
date: '2019-11-29 08:00:00'
categories:
  - 日常
  - bug解决
home_cover: ''
title: deepin 微信图片打开空白解决办法
---

在ubuntu上安装deepin微信后，打开别人发送的图片是空白的，无法显示。


网上查了下，deepin论坛给的解决办法如下：


[https://bbs.deepin.org/forum.php?mod=viewthread&tid=156161](https://bbs.deepin.org/forum.php?mod=viewthread&tid=156161)


```shell
sudo apt-get update && sudo apt-get install deepin-wine deepin-wine-helper libjpeg62-turbo:i386

```


但是我在ubuntu18.04上安装libjpeg62是提示找不到此安装包。


所以我在网上找了一个安装包手动安装后，重启微信问题解决。


libjpeg62-turbo:i386安装包地址：
[https://debian.pkgs.org/10/debian-main-i386/libjpeg62-turbo_1.5.2-2+b1_i386.deb.html](https://debian.pkgs.org/10/debian-main-i386/libjpeg62-turbo_1.5.2-2+b1_i386.deb.html)


> 注意安装i386的安装包时，如果原来机器上安装有amd64的，需要先卸载amd64的。

