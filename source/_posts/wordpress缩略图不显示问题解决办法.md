---
permalink: 2016/06/13/slove-wordpress-thumbnail-not-display
tags:
  - wordpress
layout: post
date: '2016-06-13 08:00:00'
categories:
  - 日常
  - bug解决
home_cover: ''
title: wordpress缩略图不显示问题解决办法
---

wordpress后台设置好特色图后，首页文章无法显示缩略图。
缩略图链接，提示以下错误：


```php
A TimThumb error has occured
The following error(s) occured:
GD Library Error: imagecreatetruecolor does not exist - please contact your webhost and ask them to install the GD library
Query String : src=http://my.domain.com/wp-content/themes/mytheme/img/pic/2.jpg&h=120&w=160&q=90&zc=1&ct=1
TimThumb version : 2.8.13

```


就是说当前的主机环境并没有安装GD library(图形处理模块)


这时候需要为主机安装GD模块：


在centos中可以采用yum的方式安装


```shell
[root@VM_112__250centos themes]# yum install php-gd_

```


之后根据提示安装php-gd 以及 其余依赖即可


另外：


采用yum方式安装之后，并不需要再手动更改`php.ini`文件为其添加`extension=gd.so`,
因为这种方式安装会自动的创建一个`/etc/php.d/gd.ini`文件，内容是`extension=gd.so`,
系统会自动把`/etc/php.d/`这个目录下的_`.ini`_读入`php.ini`


查找gd.so文件


```shell
[root@VM_112__250centos ~]# find / -name gd.so
/usr/lib64/php/modules/gd.so_

```


安装之后重启web服务器（这里是apache）


```shell
[root@VM_112__250centos themes]# service httpd restart_

```

