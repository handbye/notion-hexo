---
permalink: 2016/12/16/Vlan-port-summary/
tags:
  - vlan
layout: post
date: '2016-12-16 08:00:00'
categories:
  - 路由交换
  - 二层协议
home_cover: ''
title: Vlan端口类型总结
---

## ACCESS端口


收数据帧时 判断数据帧是否有标记:
有标记     和端口pvid相同收，否则丢弃
没有标记   打上端口的pvid接收


发数据帧时 判断发送的数据帧所带标记和端口的pvid是否相同
相同  去标记发送
不同  丢弃


## trunk端口


收数据帧时 判断数据帧是否有标记
有标记     接口配置pvid，经比较相同的话，剥掉标记。不同的话查看是否在允许列表中，在的话接收，不在，丢弃。接口没有配置pvid，查看允许列表。
没有标记   打上端口的pvid接收


发数据帧时 判断发送的数据帧所带标记和端口的pvid是否相同
相同  去标记发送。
不同  判断与允许通过的VLAN是否相同，相同的话发送，不同的话丢弃。


## hybird端口


收数据帧时 判断数据帧是否有标记
有标记     接收
没有标记   打上端口的pvid接收


发数据帧时
hybird端口规定的vlan是taggged属性的话 带上相应的标记发送
hybird端口规定的vlan是untaggged属性的话 发送数据帧时就去掉标记发送


Untagged和Tagged列表就是允许通过的vlan。

