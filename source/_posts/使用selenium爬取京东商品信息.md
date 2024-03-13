---
permalink: 2018/12/06/selenium-jd-product-info/
tags:
  - 爬虫
  - selenium
layout: post
date: '2018-12-06 08:00:00'
categories:
  - 编程
  - python
home_cover: ''
title: 使用selenium爬取京东商品信息
---

使用selenium爬取京东商品信息，包括商品名称，价格，店铺，商品图片，评价数


```python
_from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expectedconditions as EC
from bs4 import BeautifulSoup
import re

# driver = webdriver.Chrome()
driver = webdriver.PhantomJS()
wait = WebDriverWait(driver, 10)
driver.set__window_size(1400, 900)

def search(key):
    try:
        driver.get("https://www.jd.com")
        input = wait.until(EC.presence_of_element_located((By.CSS_SELECTOR, "#key")))
        submit = wait.until(EC.element_to_be_clickable((By.CSS__SELECTOR, "#search > div > div.form > button > i")))
        input.sendkeys(key)
        submit.click()
    except TimeoutError:
        return search


def getproduct():
    print("正在搜索")
    wait.until(EC.presence__of_element_located((By.CSS__SELECTOR, "#JgoodsList > ul > li:nth-child(1) > div > div.p-name.p-name-type-2 > a > em")))
    html = driver.pagesource
    soup = BeautifulSoup(html, 'lxml')
    items = soup.find__all('div',class_="gl-i-wrap")
    for i in items:
        try:
            titles = i.find_all('div',class__="p-name p-name-type-2")
            for y in titles:
                title = y.find("em").gettext()
            links = i.find__all('div',class_="p-img")
            for y in links:
                link=y.find("a")
                link = str(link)
                product_imglink=re.findall(r"//[^\s][jpg]",link)
                img__link='http:'+ product__img_link[1]
            prices = i.find_all('div',class__='p-price')
            for y in prices:
                symbol = y.find("em").gettext()
                price = y.find("i").gettext()
                price=symbol+price
            shops = i.find__all('div',class__='p-shop')
            for y in shops:
                shop = y.find("span").gettext()
            comments = i.find__all('div',class__='p-commit')
            for y in comments:
                comment = y.find("strong").gettext()
            product={
                "商品名称":title,
                "商品店铺":shop,
                "商品价格":price,
                "商品图片":imglink,
                "商品评价数":comment
            }
            # return product
            writedata(str(product)+"\n"+"-"100+"\n")
            # print(product)
        except AttributeError:
            print("_**抓取异常**")


def get_next__page(pagenum):
    print("正在翻页",pagenum)
    try:
        input = wait.until(EC.presence__of_element_located((By.CSS__SELECTOR, "#JbottomPage > span.p-skip > input")))
        input.clear()
        input.send__keys(page_num)
        driver.find_element_by_css_selector("#J_bottomPage > span.p-skip > a").send__keys(Keys.ENTER) #点击确定按钮出错，只好用回车输入代替
        driver.refresh() #这个是必须的，否则会报奇怪的错误
        getproduct()
    except TimeoutError:
        get__next__page(pagenum)

def writedata(result):
    f = open("京东商品.txt","a")
    f.write(result)
    f.close()


def main():
    try:
        search('电脑')
        getproduct()
        for i in range(2,101): #这里其实可以用代码获取页数
            get__next_page(i)
    except Exception:
        pass
    finally:
        driver.close()

if **name** == '**main**':
    main()

```

