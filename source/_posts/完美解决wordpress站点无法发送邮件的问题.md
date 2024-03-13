---
permalink: 2016/11/19/slove-wordpress-can't-send-email/
tags:
  - wordpress
layout: post
date: '2016-11-19 08:00:00'
categories:
  - 日常
  - bug解决
home_cover: ''
title: 完美解决wordpress站点无法发送邮件的问题
---

最近计划重新整改站点，一直有个问题困扰，那就是网站无法发送邮件，这样新用户就无法注册，自己也无法找回密码。


安装了WP email SMTP插件，虽然测试邮件可以发送成功。但注册时也无法发送邮件。


下面我为大家介绍解决办法：


1.首先测试自己的服务器或主机是否支持mail函数。测试代码如下：


```php
_<?php if (functionexists('mail')) { echo "支持mail()函数！"; } else echo "不支持mail()函数！"; ?>_

```


2.如果测试为支持，但仍然无法发送邮件，请打开php.ini修改如下配置：


```php
_[mail function]

; For Win32 only.

; http://php.net/smtp

SMTP = localhost

; http://php.net/smtp-port

smtpport = 25

; For Win32 only.

; http://php.net/sendmail-from

sendmailfrom = x.x.x.x@yy.com (此项为你发送的邮箱)

; For Unix only. You may supply arguments as well (default: "sendmail -t -i").

; http://php.net/sendmail-path

sendmailpath ="c:/sendmail/sendmail.exe -t -i"_

```


sendmail见如下分享：


http://pan.baidu.com/s/1c2FrcC4


将此文件上传到服务器。


然后打开sendmail.ini.进行如下配置：


```php
_[sendmail]

; you must change mail.mydomain.com to your smtp server

smtpserver=smtp.163.com （此项为自己的邮件SMTP地址）

smtpport=25

smtpssl=auto

; the default domain for this server will be read from the registry

; this will be appended to email addresses when one isn't provided

; if you want to override the value in the registry, uncomment and modify

;defaultdomain=mydomain.com

; log smtp errors to error.log (defaults to same directory as sendmail.exe)

; uncomment to enable logging

errorlogfile=error.log

; create debug log as debug.log (defaults to same directory as sendmail.exe)

; uncomment to enable debugging

debuglogfile=debug.log

; if your smtp server requires authentication, modify the following two lines

authusername=X.X.X.@163.com (邮件用户名)

authpassword= （邮件密码）

; if your smtp server uses pop3 before smtp authentication, modify the

; following three lines

;pop3server=

;pop3username=

;pop3password=

; to force the sender to always be the following email address, uncomment and

; populate with a valid email address. this will only affect the "MAIL FROM"

; command, it won't modify the "From: " header of the message content

forcesender=X.X.X.X@163.com （邮件地址）

; sendmail will use your hostname and your defaultdomain in the ehlo/helo

; smtp greeting. you can manually set the ehlo/helo name if required

;hostname=_

```


配置完上述文件后，重启网站应该即可解决。


注意有的主题可能不支持邮件的发送，建议更换。

