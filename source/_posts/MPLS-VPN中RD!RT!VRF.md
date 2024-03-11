---
permalink: 2016/11/26/MPLS-VPN-RD-RT-VRF
tags:
  - MPLS
layout: post
date: '2016-11-26 08:00:00'
categories:
  - 路由交换
  - MPLS
home_cover: ''
title: MPLS-VPN中RD/RT/VRF
---

RD (Route Distinguisher)：

1. 用于标识PE上不同VPN实例，其主要作用是实现VPN实例之间地址复用，与IP地址一起构成12 Bytes的VPNv4地址。
2. RD与路由一起被携带在BGP Update报文中发送给对端。
3. RD不具有选路能力，不影响路由的发送与接受。
4. RD用来区分本地VRF，本地有效。

RT (Route Target)：

1. RT是VPNv4路由携带的一个重要属性，它决定VPN路由的收发和过滤，PE依靠RT属性区分不同VPN之间路由。
2. 当从VRF表中导出VPN路由时，要用Export RT对VPN路由进行标记。
3. 当往VRF表中导入VPN路由时，只有所带RT标记与VRF表中任意一个Import RT相符的路由才会被导入到VRF表中。

什么是VRF呢？


VRF：Virtual Routing and Forwarding，翻译成虚拟路由及转发，它是一种VPN路由和转发实例。一台PE路由器，由于可能同时连接了多个VPN用户，这些用户（的路由）彼此之间需要相互隔离，那么这时候就用到了VRF，PE路由器上每一个VPN都有一个VRF。PE路由器除了维护全局IP路由表之外，还为每个VRF维护一张独立的IP路由表，这张路由表称为VRF路由表。


要注意的是全局IP路由表，以及VRF路由表都是相互独立或者说相互隔离的。 一旦在PE路由器上创建了一个VRF，我们就可以将特定的接口（物理或逻辑的）放入这个VRF，那么这个接口将不再属于全局IP路由表或其他任何VRF，只为该VRF服务。

