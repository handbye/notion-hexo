---
permalink: 2018/06/13/NATserver-DNAT
tags:
  - NAT Server
  - 目的NAT
layout: post
date: '2018-06-13 08:00:00'
categories:
  - 网络安全
  - NAT
home_cover: ''
title: NAT server和目的NAT
---

NAT可以分为两大类：


1.基于源IP地址的NAT
2.基于目的IP地址的NAT


按照功能不同，基于目的IP地址的NAT server和目的NAT：
**NAT Server**：主要应用于实现私网服务器以公网IP地址对外提供服务的场景。
**目的NAT**：主要应用于实现手机用户上网时，手机的缺省WAP网关与所在地运营商的实际WAP网关不一致，导致需要修改报文的目的网关地址的场景


## NAT server


**NAT Server是最常用的基于目的地址的NAT** 。当内网部署了一台服务器，其真实IP是私网地址，但是希望公网用户可以通过一个公网地址来访问该服务器，这时可以配置NAT Server，使设备将公网用户访问该公网地址的报文自动转发给内网服务器。


NAT Server功能使得内部服务器可以供外部网络访问。外部网络的用户访问内部服务器时，NAT将请求报文的目的地址转换成内部服务器的私有地址。对内部服务器回应报文而言，NAT还会自动将回应报文的源地址（私网地址）转换成公网地址。


NAT Server可以通过静态IP（即global IP地址）和动态IP（即接口IP地址）两种方式实现地址转换。当通过global IP地址配置NAT Server后，再通过基于接口地址的方式配置NAT Server，当被借用的接口的地址与global IP地址相同时，二者冲突，基于接口方式的NAT Server不生效。


## 目的NAT


在移动终端访问无线网络时，如果其缺省WAP网关地址与所在地运营商的WAP网关地址不一致时，可以在终端与WAP网关中间部署一台设备，并配置目的NAT功能，使设备自动将终端发往错误WAP网关地址的报文自动转发给正确的WAP网关


手机用户需要通过登录WAP（Wireless Application Protocol）网关来实现上网的功能。目前，大量用户使用直接从国外购买的手机，这些手机出厂时，缺省设置的WAP网关地址与本国WAP网关地址不符，且无法自行修改，从而导致用户不能移动上网。


为解决这一问题，无线网络中，在WAP网关与用户之间部署USG。通过在设备上配置目的NAT功能，使这部分手机用户能够正常获取网络资源。


> 目的NAT用的比较少，大致了解下就可以了。


更多详细信息和配置请参考：
http://support.huawei.com/hedex/hdx.do?docid=EDOC1000038797&lang=zh
有关NAT SERVER和目的NAT的说明。

