---
permalink: 2016/05/08/slove-phpmyadmin-page-null
tags:
  - phpmyadmin
layout: post
date: '2016-05-08 08:00:00'
categories:
  - 日常
  - bug解决
home_cover: ''
title: 完美解决安装phpmyadmin后打开页面空白的问题
---

今天在Ubuntu上搭建了Apache+php+mysql的环境，安装了phpmyadmin后页面打开空白，网上找不到解决方法，最后还是在国外的一个网站上找到了解决办法。


首先在phpmyadmin文件夹下的index.php页面开头加入


```php
ini_set("display_errors", "On");
error_reporting(E__ALL | ESTRICT);_

```


这两句话，再次打开localhost/phpmyadmin后就会报错。
如果提示中有_`requireonce GETTEXTINC;`_则


```php
sudo apt-get install php-gettext

```


安装php-gettext然后就可以了。


如果还是报错执行下面这句话修改phpmyadmin的权限


```php
pkexec chmod 755 -R /etc/phpmyadmin

```


然后就可以访问了。

