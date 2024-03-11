---
permalink: 2018/03/15/web-login-esnp-USG6000V
tags:
  - ensp
  - USG
layout: post
date: '2018-03-15 08:00:00'
categories:
  - 网络安全
  - 防火墙
home_cover: ''
title: 使用web登陆esnp模拟器上的USG6000V防火墙
---

华为的USG系列防火墙默认是开启了web登陆的，我们可以使用浏览器登陆到防火墙中对其进行一些配置。如果没有真实设备，但又想体现下使用web管理防火墙怎么办呢？
别担心，华为的ENSP模拟上的USG6000V防火墙也是就可以用浏览器来登录管理的，下面就介绍下怎么来登录。

1. 打开ensp，并打开一台防火墙，然后打开一个cloud

拓扑连接如下：


![d220e8527a0efc62cbeb08d6c5bb46c1-373x196.png](../post_images/285102571c8bbcc49087d066eb7312bc.png)


cloud的配置如下：


![5d893d4041be0240a359d76f57966bd9-768x521.png](../post_images/f853d91efe6e97371b0edc00cf10574e.png)


> 关于如何使用cloud桥接本地网卡的说明参考：http://www.cnblogs.com/handbye/p/7709427.html

1. 保证电脑可以ping通防火墙的G0/0/0接口，华为USG防火墙的默认管理接口为G0/0/0，默认管理地址为：192.168.0.1

管理接口的默认配置如下：


```shell
interface GigabitEthernet0/0/0
 undo shutdown
 ip binding vpn-instance default
 ip address 192.168.0.1 255.255.255.0
 service-manage http permit
 service-manage https permit
 service-manage ping permit
 service-manage ssh permit
 service-manage snmp permit
 service-manage telnet permit
 service-manage netconf permit

```


我的电脑本地连接5地址为：192.168.0.100


![143fd5f1e023f12be430f80aa9233118-373x110.png](../post_images/bf63eafdb7e1d95d7af7a8f07330b24c.png)


我是可以ping通192.168.0.1的。

1. 打开浏览器，输入：https://192.168.0.1:8443/ 即可打开防火墙登陆界面。

![73f3acdc4df7e00aa603741015429005-768x397.png](../post_images/ad3d5db56e67ccce9ef0b568f95e30d6.png)

