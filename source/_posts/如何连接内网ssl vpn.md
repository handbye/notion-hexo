---
permalink: 2022/06/15/connect-Intranet-ssl-vpn
tags:
  - sslvpn
layout: post
date: '2022-06-15 08:00:00'
categories:
  - 网络安全
  - VPN
home_cover: 'https://cdn.jsdelivr.net/gh/handbye/images/picgo2022-06-15-10-30-29.png'
title: 如何连接内网ssl vpn?
---

## 背景


在某次HW时，我们在内网发现了一个防火墙web登录弱口令，在登录后查看配置时，发现此防火墙可通内网核心管理网段，拓扑大概如下：


![picgo2022-06-15-11-50-50.png](../post_images/eab55c4dfda131fe36f19cb44974fc1d.png)


此时我们得设法让攻击机能够访问此核心网段。我们想了如下几个方法：

1. 在防火墙上添加到核心网段的路由
2. 做端口映射
3. 启用ssl vpn，将目标网段加到ssl vpn中。

先说下这几种方法的利弊，第一种方法最为简单，但是无法确定目标网段的网关设备在哪，有可能网关设备上没有到攻击机(跳板机)网段的回程路由。第二种方法做端口映射，如果不确定核心网段开放的端口也会很麻烦，且不利于fscan等工具做信息收集。第三种方法最为方便，一旦ssl vpn建立，就相当于攻击机已经处于核心网段之中了，这时候无论是信息收集还是漏洞利用都很方便，但是此防火墙处于内网，如何连接其ssl vpn是个问题。


## socks5代理方式连接失败


当在防火墙上打开ssl vpn后，查看其登录页面如下。


![picgo2022-06-15-13-58-03.png](../post_images/7b94e656130e6a053f8d37e704b971b7.png)


按照其提示我们下载了ssl vpn客户端，并将其加入profile规则中，让其走socks5去连接ssl vpn服务器端。


![picgo2022-06-15-14-17-09.png](../post_images/a49085248b2ccad377544ea506a62454.png)


![picgo2022-06-15-15-26-27.png](../post_images/7f86b88abb66ead913deab8235e486fa.png)


此种方法尝试后发现，sslvpn在认证通过后立马又会掉线，查看连接日志发现是udp协商未通过。


![picgo2022-06-15-15-27-53.png](../post_images/f94089fa02ca1c7b135bb1658447235b.png)


![picgo2022-06-15-15-28-07.png](../post_images/b3e1fd62f6017b1ec17c285b188c2feb.png)


由于socks5代理不支持UDP，所以UDP传输失败导致了ssl vpn无法正常建立。但是不是所有厂家的ssl vpn都不支持socks5代理的方式去连接，例如华为的ssl vpn就可以在其客户端上配置代理去连接。


![picgo2022-06-15-15-34-49.png](../post_images/b3643cc1583c3ee86994ca48c67eba74.png)


所以在实际环境中可以先去尝试使用socks5代理去连接，不成功的话再采取端口转发的方法。


## 端口转发解决问题


上面提到了由于socks5不支持UDP传输导致了ssl vpn建立失败，那我们分析下再其建立过程种UDP协商时使用的端口。


![picgo2022-06-15-16-47-38.png](../post_images/1e887a8fccad0db8886da8d13b0541ca.png)


![picgo2022-06-15-16-47-55.png](../post_images/063ce916ad03a8b249793e5f95b74f91.png)


可以看到使用的是442端口，那么我们就想办法把UDP442端口转发出来。


这次使用的端口转发工具是FRP,其支持tcp，udp的端口转发。


这里就贴一下FRP的配置吧：


公网VPS的 frps.ini


```text
_[common]
bindaddr = 0.0.0.0
bindport = 10991_

```


内网可出网机器 frpc.ini


```text
_[common]
serveraddr = 1.14.47.152
serverport = 10991
[tcp]      
type = tcp 
localport = 8072        
remoteport = 8072
[udp]      
type = udp
localport = 8073         
remoteport = 8073        
[forward1]         
type = tcp
localip = 127.0.0.1    
localport = 8072        
remoteport = 12020
[forward2]         
type = udp
localip = 127.0.0.1    
localport = 8073        
remoteport = 442_

```


内网可出网机器 frps.ini


```text
_[common]
bindaddr = 0.0.0.0
bindport = 21_

```


内网不出网机器 frpc.ini


```text
_[common]
serveraddr = 172.31.32.33  //内网可出网机器的ip
serverport = 21

[tcp]
type = tcp             
remoteport = 8072     
localip = 192.168.254.203 //sslvpn的ip地址
localport = 8081  //sslvpn的http端口

[udp]
type = udp
remoteport = 8073
localip = 192.168.254.203 //sslvpn的ip地址
localport = 442 //sslvpn的udp端口_

```


这样基于可以把ssl vpn的http端口和udp端口全部转发出来了，再攻击机直接连接转发出来的端口即可。


![picgo2022-06-15-17-08-00.png](../post_images/d21175b320bba4700172dcca0da2e295.png)


可以看到vpn已成功建立连接，并分配了网卡。


![picgo2022-06-15-17-08-30.png](../post_images/5e0b810ab89179baeb673210956e2008.png)


![picgo2022-06-15-17-08-47.png](../post_images/0b5dd3aa8c9a38629d14f2fec052435b.png)


这样就可以随意访问核心网段的地址了，ping也是没问题的。


![picgo2022-06-15-17-19-44.png](../post_images/804d4870808bc1fab217f29beb23e945.png)


## 参考


[使用Frp实现无公网地址（家庭宽带）环境下的SSL VPN部署](https://bbs.sangfor.com.cn/forum.php?mod=viewthread&tid=57000)

