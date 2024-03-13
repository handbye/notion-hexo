---
permalink: 2018/12/02/python-toutiao-pic/
tags:
  - 爬虫
layout: post
date: '2018-12-02 08:00:00'
categories:
  - 编程
  - python
home_cover: ''
title: python抓取头条街拍图片
---

## 爬取头条街拍图片


```python
#抓取头条街拍图片
#抓取地址：https://www.toutiao.com/search/?keyword=%E8%A1%97%E6%8B%8D

import requests
from urllib import parse
from requests.exceptions import ConnectionError
from bs4 import BeautifulSoup
import re
import json
import os
from hashlib import md5

proxies={
    "http":"14.29.2.40:80",
    "http":"221.7.255.167:80",
    "http":"112.253.22.161:80"
}
headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36'
}

def get_index__page(offset,kw):
    baseUrl = 'https://www.toutiao.com/searchcontent/?'
    data = {
        'offset': offset,
        'format': 'json',
        'keyword': kw,
        'autoload': 'true',
        'count': '20',
        'curtab': 1,
        'from': 'searchtab',
        'pd': 'synthesis'
        }
    dataUrl=parse.urlencode(data)
    url = baseUrl+dataUrl
    try:
        rsp = requests.get(url,headers=headers,proxies=proxies)
        if rsp.statuscode==200:
            return rsp.text
        else:
            return None
    except ConnectionError:
        print("请求错误")

def parse__index__page(indextext):
    json__data = json.loads(index__text)
    pageurls=[]
    if json__data and "data" in json__data.keys():
        for dic in jsondata.get("data"):
            page__url=dic.get('article_url')
            page_urls.append(page__url)
        for i,value in enumerate(pageurls):
            if value ==None:
                pageurls.remove(value)
        del pageurls[0:2] #前两台为广告，删除
        return pageurls


def get__page(page__url):
    try:
        rsp = requests.get(pageurl,headers=headers,proxies=proxies)
        if rsp.statuscode==200:
            return rsp.text
        else:
            return None
    except ConnectionError:
        print("请求错误")


def parse__page(page__text):
    soup = BeautifulSoup(pagetext,'lxml')
    image__title=soup.select('title')[0].get__text
    imagepattern=re.compile('gallery: JSON.parse\("(.__)"\)|articleInfo:(._)',re.S)
    image_url=re.search(image__pattern,pagetext)
    try:
        if imageurl.group(1)==None:
            image__url__data=imageurl.group(0)
            image__url_data_1=re.search('content:(.)',image__urldata)
            image__url_data_2=re.findall(r'[a-zA-z]+://[^\s][^&]',str(image_url_data_1.group()))
            for i,image in enumerate(image_url__data2):
                dowloadimage(image)
            return {
                "title":imagetitle
            }
        else:
            image__url__data=json.loads(imageurl.group(1).replace("\\",""))
            sub__images = image_url_data.get('sub__images')
            images = [item.get('url') for  item in subimages]
            for image in images:
                dowloadimage(image)
            return {
                "title": imagetitle
            }
    except AttributeError:
        print("参数错误")


def dowloadimage(url):
    print("正在下载",url)
    try:
        rsp = requests.get(url)
        if rsp.statuscode==200:
            saveimage(rsp.content)
        return None
    except ConnectionError:
        return "连接错误"


def saveimage(content):
    imgpath = '头条街拍图片'  # 图片的路径
    t__img__name=md5(content).hexdigest()
    try:
        if not os.path.exists(imgpath):  # 先判断路径是否存在
            os.makedirs(imgpath)
        t__img_name = '{}{}{}{}'.format(img_path, os.sep, t__imgname, ".jpeg")  # 组合成图片名称
        print(t__img_name)
        f=open(t_img_name,"wb")
        f.write(content)
        f.close()
    except ConnectionError:
        print("error")


def main():
    index_text=get__indexpage(20, "街拍")
    page__urls = parse_index_page(index__text)
    for i in range(len(pageurls)):
        try:
            page__url = page_urls[i]
            page_text=get__page(pageurl)
            parse__page(page_text)
        except TypeError:
            pass

if **name** == '**main**':
    main()

```

