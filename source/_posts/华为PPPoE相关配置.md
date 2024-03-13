---
permalink: 2016/12/08/huawei-PPPoE-config/
tags:
  - PPPoE
layout: post
date: '2016-12-08 08:00:00'
categories:
  - 路由交换
  - 二层协议
home_cover: ''
title: 华为PPPoE相关配置
---

![20190224214246.png](../post_images/f4a411f6f80b97be45e50a18755108db.png)


客户端


```shell
dialer-rule

dialer-rule 1 ip permit

#

ip route-static 0.0.0.0 0.0.0.0 Dialer1

interface Dialer1

link-protocol ppp

ppp chap user 

ppp chap password cipher 

ip address ppp-negotiate

dialer user hcie

dialer bundle 1

dialer-group 1

#

interface GigabitEthernet0/0/1

pppoe-client dial-bundle-number 1 on-demand

#

```


服务器端：


```shell
#

ip pool pppoepool

gateway-list 10.1.12.254

network 10.1.12.0 mask 255.255.255.0

#

aaa

local-user  password cipher 

local-user hcie service-type ppp

#

#

interface Virtual-Template1

ppp authentication-mode chap

remote address pool pppoepool

ip address 10.1.12.2 255.255.255.0

#

interface GigabitEthernet0/0/0

pppoe-server bind Virtual-Template 1

#

```

