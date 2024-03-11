---
permalink: 2016/12/18/one-img-STP-BPDU
tags:
  - BPDU
layout: post
date: '2016-12-18 08:00:00'
categories:
  - 路由交换
  - 二层协议
home_cover: ''
title: 一张图详解STP配置BPDU
---

![eb8b3905gy1fhke1w1vr4j20gz0e8js2.jpg](https://ws1.sinaimg.cn/large/eb8b3905gy1fhke1w1vr4j20gz0e8js2.jpg)


配置BPDU内容：


![eb8b3905gy1fhkefmbvs5j20mi0eot9u.jpg](https://ws1.sinaimg.cn/large/eb8b3905gy1fhkefmbvs5j20mi0eot9u.jpg)


最重要的四个参数：

- Root Identifier:发送此配置BPDU的交换机所认为的根交换机的交换机标识
- Root Path Cost:从发送此配置BPDU的交换机到达根交换机的最短路径总开销，含交换机根端口的开销，不   含发送此配置BPDU的端口的开销
- Bridge Identifier:发送此配置BPDU的交换机的交换机标识
- Port Identifier:发送此配置BPDU的交换机端口的端口标识

首先应该了解交换机在STP选举的过程中有两个参数：全局参数和端口参数。


![0a8519f2a03cd2db47137510bf04c7b1.png](../post_images/9b1c3105cfceab2f22a7a660209df5cd.png)


**全局参数**


![eb8b3905gy1fhkf2hcky1j20d107ct97.jpg](https://ws1.sinaimg.cn/large/eb8b3905gy1fhkf2hcky1j20d107ct97.jpg)


**端口参数**


端口参数是为全局参数服务的，最终的选举结果直观表现就为全局参数，当交换机收到一个配置BPDU后，首先将端口参数和BPDU中的参数作比较，如果收到的BPDU中的参数比自己的端口参数优，则改变端口参数。BPDU对比完毕，参数改变后开始计算根交换机，根端口，根路径开销。计算完毕后，对应改变交换机的全局参数，至此，每个交换机都知道谁是根桥，哪个端口是根端口以及根路径开销。这个过程计算完毕后，交换机将在非根端口中依据全局参数和端口参数计算指定端口，计算完毕后更新指定端口参数。


（备注：不仅交换机要知道谁是根桥谁是指定桥，每个端口也要知道）


根交换机、根端口和根路径开销计算过程：


1.根据所有的端口上记录的参数，依次比较Designated Root，Designated Cost和端口Cost之和，Designated Bridge和Designated Port，从中选出一个记录了最优参数的端口，并且此端口上记录的Designated Root要比交换机自身的Bridge Identifier（交换机标识）更优先，此端口即为根端口；
2.选择出此端口之后，更新交换机全局参数Designated Root为根端口记录的Designated Root；更新交换机全局参数Root Path Cost为根端口记录的Designated Cost与根端口的Port Cost之和；
3.如果任何端口记录的Designated Root参数都不比交换机自身的Bridge Identifier更优先，则交换机全局参数Designated Root设置为交换机自身的Bridge Identifier；交换机全局参数Root Path Cost设置为0。


确定一个端口可以成为指定端口之后，交换机需要修改指定端口的参数，修改规则如下：


Designated Root设置为交换机全局参数Designated Root；


Designated Cost设置为交换机全局参数Root Path Cost；


Designated Bridge设置为交换机自身的Bridge Identifier；


Designated Port设置为端口自身的Port Identifier。

