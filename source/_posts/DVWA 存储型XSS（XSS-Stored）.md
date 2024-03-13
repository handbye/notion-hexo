---
permalink: 2019/05/13/dvwa-xss-stored/
tags:
  - XSS
layout: post
date: '2019-05-13 08:00:00'
categories:
  - web安全
  - DVWA
home_cover: ''
title: DVWA 存储型XSS（XSS-Stored）
---

## 存储型XSS简介


存储型XSS也叫持久型XSS，这种XSS需要服务端的参与，它与反射型XSS的区别在于XSS代码是否持久化（硬盘，数据库）。反射型XSS过程中后端服务器仅仅将XSS代码保存在内存中，并未持久化，因此每次触发反射性XSS都需要由用户输入相关的XSS代码；而持久型XSS则仅仅首次输入相关的XSS代码，保存在数据库中，当下次从数据库中获取该数据时在前端未加字串检测和excape转码时，会造成XSS，而且由于该漏洞的隐蔽性和持久型的特点，在多人开发的大型应用和跨应用间的数据获取时造成的大范围的XSS漏洞，危害尤其大。


存储型XSS最容易在留言板，个性签名等用户输入的文本会存储到数据库中的地方发生。


## Low等级


此等级和反射型XSS一样没有做任何防护，直接输入js代码即可执行，需要注意的是，此js代码会存储进数据库中，每当用户访问此页面就会执行一次XSS。


![20190518100040.png](../post_images/c118b7ab0bb7d8291f796ed911d10844.png)


提交到留言板中就会执行xss，并且每次刷新此页面就会被执行一次。


![20190518100117.png](../post_images/d0a43139fe78bf84081ee3a47ab87a44.png)


## Medium等级


提交Low等级的js代码，发现服务器将`<script>`标签过滤了。


![20190518100326.png](../post_images/dace6c4cb0da0c917466aec545e05649.png)


那不使用`<script>`标签，换用其它标签试试。


![20190518100453.png](../post_images/f9392d7f2dea78907586a28ef8cb5d01.png)


发现输出结果为空，且没有执行XSS。


![20190518101342.png](../post_images/abed67b0e4a73bb0ea11e8a6bc5ca3b8.png)


查看源码发现：


![20190518101522.png](../post_images/4db0a327555d8245b20f0ddc3544761b.png)


与Low级的相比，name值的处理增加了一个_`strreplace()`_ 函数，用来将字符串中含有`<script>`的字符串替换为空字符，从而稍微有点过滤作用；message值的处理中，在其第二次调用`trim()`函数时还内嵌了两个函数，先是使用`addslashes()` 函数、返回在预定义字符之前添加反斜杠的字符串，然后调用_`striptags()`_函数来剥去字符串中的HTML、XML 以及 PHP 的标签，在最后再调用`htmlspecialchars()` 函数、把预定义的字符转换为 HTML 实体，对输入的内容先进行HTML的编码然后再存储进服务器中，从而使message的SQL和XSS漏洞几乎不存在。


但是此等级对`name`的输入过滤不足，可以进行利用。


在输入name时，发现只允许输入10个字符，此时需要构造不超过10个字符的js代码或者抓包修改name输入进行上传，我使用的是第二种方法。


![20190518103202.png](../post_images/9fcfab0eebf4822c875b51af141e6b31.png)


发现js代码执行成功，注意`<script>`中S大写。


## High等级


查看源码发现，在Medium等级的基础上，对name的输入也增加过滤，使用正则匹配`<script>`并进行替换，且大小写都替换。


![20190518103529.png](../post_images/ed1a3e0d8db4d4c58374095f6dc1e359.png)


在Medium基础上重新构造js代码，不包含`<script`即可。


![20190518104502.png](../post_images/210275c9356ff4dd37e7f9c3d5e3b590.png)


但是并没有执行XSS ，查看源码发现插入的js代码变为了如下形式：


![20190518104548.png](../post_images/b68cc0c98b3d718f3fe6f8d21d3fc540.png)


尝试将输入的js代码进行url编码后重新发送：


![20190518110005.png](../post_images/e93145d797fd13df05096d39bd676668.png)


发现插入成功， xss执行。


![20190518110042.png](../post_images/f555f095f3d7e7b2ee314078a7b818bc.png)


## Impossible等级


![20190518110210.png](../post_images/2d922d1a53e39e6b0902da466f662cac.png)


此等级对name和message的输入做了同等过滤，已无法进行xss。

