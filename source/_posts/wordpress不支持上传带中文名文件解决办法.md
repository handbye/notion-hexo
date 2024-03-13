---
permalink: 2016/12/09/wordpres-upload/
tags:
  - wordpress
layout: post
date: '2016-12-09 08:00:00'
categories:
  - 日常
  - bug解决
home_cover: ''
title: wordpress不支持上传带中文名文件解决办法
---

WordPress目录下找到wp-admin/includes/file.php这个文件。在wp-admin/includes/file.php文件中查找


```php
$new_file = $uploads['path'] . "/$filename";

```


替换成下面的:


```php
$new_file = $_uploads['path'] . "/".date("YmdHis").floor(microtime()1000).".".$ext;_

```


这样就可以实现wordpress上传图片自动重命名了。


然后在编辑图片中改成你想要的中文名字就好，经测试这一方法比较好用，其他方法暂不可用。


![5abc820708feb.png](../post_images/b06d020e08e0e1cb5b2ac57f2cf34618.png)

