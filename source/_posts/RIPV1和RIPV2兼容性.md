---
permalink: 2016/11/24/RIPV1-RIPV2-compatibility
tags:
  - rip
layout: post
date: '2016-11-24 08:00:00'
categories:
  - 路由交换
  - 路由协议
home_cover: ''
title: RIPV1和RIPV2兼容性
---

![5ab8e61c45820.png](../post_images/c15d816de51f74ab4ddc0f95e75364fb.png)


RIPV2中有个字段Unused


如果运行了RIPV2协议的路由器收到了更新报文的版本字段指出RIP的版本为1,但所有未使用的字段(UNUSED FIELD)的所有位都被设置为1,那么这个更新报文将被丢弃;


如果版本改字段设置大于1,此路由器收到的RIPV1报文中定义为未使用的字段将被忽略,并且处理这个消息。


结果,像RIP V2协议这样新版本的协议就可以向后兼容RIP-V1.


＂兼容性开关＂,用来允许版本1和版本2之间的互操作:


RIP-1——只有RIPV1的消息传送:
RIP-1兼容性——使RIPV2使用广播方式代替组播方式来通告消息,以便RIPV1可以接收它们;


RIP-2——RIP V2协议使用组播方式通告消息到目的地址224.0.0.9


NONE——不发送消息

