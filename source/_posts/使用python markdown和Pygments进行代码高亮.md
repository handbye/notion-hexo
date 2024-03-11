---
permalink: 2019/01/11/use-python-markdown-and-Pygments
tags:
  - Pygments
layout: post
date: '2019-01-11 08:00:00'
categories:
  - 编程
  - python
home_cover: ''
title: 使用python markdown和Pygments进行代码高亮
---

在学习django的时候，需要用到markdown编辑器，当使用编辑器写出来文章后需要在前端渲染出来，这时候就需要用到python-markdown这个库了。


## python-markdown

- 安装

```python
pip install markdown

```

- 使用

```python
import markdown
html = markdown.markdown(your_text_string)

```

- 扩展
当要用到一些markdown的扩展时，需要进行如下配置：

```python
markdown.markdown(your_text_string,extensions=['markdown.extensions.extra',
                                              'markdown.extensions.codehilite',
                                              'markdown.extensions.tables',
                                              'markdown.extensions.toc'])

```


但是启用扩展后代码依旧无法高亮，那是因为我们没有给前端页面应用相应的css样式， python-markdown是不带代码高亮样式的。


## Pygments


pygments是一款可以自动生成代码高亮css的工具。

- 安装

```python
pip install pygments

```

- 使用
pygmentize -S native -f html > pygments.css

其中**native**为代码高亮样式名称


可以使用的代码高亮样式可以去官网查看：


http://pygments.org/demo


最后将生成好的css文件放入项目所在文件夹，然后在对应的前端页面引入即可。

