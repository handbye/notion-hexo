---
permalink: 2018/12/01/python-maoyan-movie-top100/
tags:
  - 爬虫
  - 猫眼电影
layout: post
date: '2018-12-01 08:00:00'
categories:
  - 编程
  - python
home_cover: ''
title: python爬取猫眼电影top100
---

## 代码：


```python
_# 爬取猫眼电影榜单top100
#地址：http://maoyan.com/board/4

import requests
from bs4 import BeautifulSoup
import re

titles = []  # 电影名字
stars = []  # 主演
times = []  # 上映时间
scores = []  # 评分

def getList():
    for page in range(0,100,10):
        url = "http://maoyan.com/board/4"+"?offset="+str(page)
        headers= {
            "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36"
        }
        rsp = requests.get(url,headers=headers)
        html = rsp.text
        soup = BeautifulSoup(html,'lxml')
        dd = soup.findall('dd')
        for i in dd:
            movie__title = i.find(class__='name').gettext()
            #print(title.gettext())
            titles.append(movietitle)
            movie__star = i.find(class__='star').gettext()
            movie__star=re.findall('主演：(.)',movie__star)
            stars.append(moviestar)
            movie__time = i.find(class__='releasetime').gettext()
            movie__time=re.findall('上映时间：(.)',movie__time)
            times.append(movietime)
            movie__score_interger = i.find(class_='integer').get_text()
            movie_score_num = i.find(class_='fraction').get_text()
            movie_score=movie_score_interger+movie__scorenum
            scores.append(moviescore)


def writetxt():
    file = open('maoyan.txt',"w")
    for i in range(100):
        onewrite = str(titles[i])+"--- "+str(times[i])+"--- "+str(scores[i])+"--- "+str(stars[i])+"\n"
        file.write(onewrite)
    file.close()

if_ **name** == '**main**_':
    getList()
    writetxt()_

```

