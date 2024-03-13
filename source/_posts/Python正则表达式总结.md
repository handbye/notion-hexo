---
permalink: 2019/04/02/Python-regex-summary/
tags:
  - python
  - 正则表达式
layout: post
date: '2019-04-02 08:00:00'
categories:
  - 编程
  - python
home_cover: ''
title: Python正则表达式总结
---

## 常用匹配规则


| **模式**  | **描述**                                           |
| ------- | ------------------------------------------------ |
| \w      | 匹配字母、数字及下划线                                      |
| \W      | 匹配不是字母、数字及下划线的字符                                 |
| \s      | 匹配任意空白字符，等价于［\t\n\r\f]                           |
| \S      | 匹配任意非空字符                                         |
| \d      | 匹配任意数字，等价于［0-9]                                  |
| \D      | 匹配任意非数字的字符                                       |
| \A      | 匹配字符串开头                                          |
| \Z      | 匹配字符串结尾，如果存在换行，只匹配到换行前的结束字符串                     |
| \z      | 匹配字符串结尾，如果存在换行，同时还会匹配换行符                         |
| \G      | 匹配最后匹配完成的位宣                                      |
| \n      | 匹配一个换行符                                          |
| \t      | 匹配一个制表符                                          |
| ^       | 匹配一行字符串的开头                                       |
| $       | 匹配一行字符串的结尾                                       |
| .       | 匹配任意字符，除了换行符，当re.DOTALL 标记被指定时，贝lj可以匹配包括换行符的任意字符 |
| [ ...]  | 用来表示一组字符，单独列出， 比如［ amk ］匹配a 、m 或k                |
| [ ^...] | 不在［］中的字符，比如（^abc ］匹配除了a 、b 、c 之外的字符              |
| *       | 匹配0 个或多个表达式                                      |
| +       | 匹配1 个或多个表达式                                      |
| ?       | 匹配0 个或l 个前面的正则表达式定义的片段，非贪婪方式                     |
| {n}     | 精确匹配n 个前面的表达式                                    |
| {n,m}   | 匹配n 到m 次由前面正则表达式定义的片段，贪婪方式                       |
| a|b     | 匹配a 或b                                           |
| ( )     | 匹配括号内的表达式，也表示一个组                                 |


## 常用方法


### match()


match （）方法会尝试从字符串的起始位置匹配正则表达式，如果匹配，就返回匹配成功的结果；如
果不匹配，就返回none 。


例：


```python
_import re
content ='Hello 1234567 WorldThis is a Regex Demo'
print(len(content))
result = re.match ('^Hello\s(\d+)\sWord',content)
print(result.group()) # 默认为group(0) 即匹配能匹配到的所有内容
print(result.group(1)) # group(1)可匹配正则表达式中第一个（）中的内容
print(result.span())_

```


运行结果：


```python
<_sre.SRE_Match object; span=(O, 19), match ='Hello 1234567 World'>
Hello 1234567 World
1234567
(0,19)

```


_**SREMatch对象有两个方法：**_

- group()方法可以输出匹配到的内容
- span()方法可以输出匹配的范围

### search()


前面提到过， match （）方法是从字符串的开头开始匹配的，一旦开头不匹配，那么整个匹配就失败
了。我们看下面的例子：


例：


```python
import re
content= 'Extra stings Hello 1234567 World This is a Regex Demo Extra stings '
result = re.match ('Hello._?（＼d+）._?Demo',content)
print(result)

```


运行结果：


```python
None

```


这里的字符串以Extra 开头，但是正则表达式以Hello 开头，整个正则表达式是字符串的一部分，但是这样匹配是失败的。


因为match ()方法在使用时需要考虑到开头的内容，这在做匹配时并不方便。它更适合用来检测某个字符串是存符合某个正则表达式的规则。


**这里就有另外一个方法search ()，它在匹配时会扫描整个字符串，然后返回第一个成功匹配的结果。也就是说，正则表达式可以是字符串的一部分，在匹配时， search()方法会依次扫描字符串，直到找到第一个符合规则的字符串，然后返回匹配内容，如果搜索完了还没有找到，就返回None 。**


把上面的代码改为：


```python
import re
content= 'Extra stings Hello 1234567 World This is a Regex Demo Extra stings '
result = re.search ('Hello._?（＼d+）._?Demo',content)
print(result)

```


运行结果：


```python
1234567

```


### findall()


search ()方法的用法，它可以返回匹配正则表达式的第一个内容，但是如果想要获取匹配正则表达式的所有内容，那该怎么办呢？这时就要借助findall ()方法了。


findall ()方法会搜索整个字符串，然后返回匹配正则表达式的所有内容。


例：


这里有一段待匹配的HTML文本：


```html
html ＝`＜div id="songs-list">
<h2 class="title">经典老歌</h2>
<p class="introduction">经典老歌列表</p>
<ul id="list" class="list-group">
<li data-view="2">一路上有你</li>
<li data-view="7">
    <a href ＝"/2.mp3" singer ＝"任贤齐">沧海一卢笑</a>
</li>
<li data-view="4" class="active">
	<a href ＝"/3.mp3" singer ＝"齐泰">往事随风</a>
</li>
<li data-view ＝"6"＞
    <a href ="/4.mp3" singer ＝"beyond">光辉岁月</a> 
</li>
</ul>
</div>`

```


python代码：


```python
results = re.findall('<li._?href＝"(.＊?)"._?singer="（._?）"＞（._?）</a>',html,re.S)
print(results)
print(type(results))
for result in results:
	print(result)
	print(result[o], result[1], result[2])

```


运行结果：


```python
［（'/2.mp3'，'任贤齐'， '沧海一卢笑'）,（'/3.mp3'，'齐泰'，'往事随风'）,（'/4.mp3', 'beyond'，'尤辉岁月'）］
<class 'list'>
（'/2.mp3'，'任贤齐'， '沧海一卢笑'）
（'/3.mp3'，'齐泰'，'往事随风'）
（'/4.mp3', 'beyond'，'尤辉岁月'）

```


可以看到，返回的列表中的每个元素都是元组类型，我们用对应的索引依次取出即可。


如果只是获取第一个内容，可以用search ()方法。当需要提取多个内容时，可以用于findall()方法。


### complie()


complie方法可以将正则字符串编译成正则表达式对象，以便在后面的匹配中复用。


例：


```python
import re
contentl ='2016 12 15 12:00'
content2 ='2016-12-17 12:55'
pattern = re.compile('\d{2}:\d{2}')
resultl = re.sub(pattern, '’,contentl)
result2 = re.sub(pattern, '',content2)
print(reslt1, result2)

```


运行结果：


```python
2016-12-15  2016-12-17

```


例如，这里有2个日期，我们想分别将2个日期中的时间去掉，这时可以借助**sub ()**方法。该方法的第一个参数是正则表达式，但是这里没有必要重复写32个同样的正则表达式，此时可以借助**compile ()**方法将正则表达式编译成一个正则表达式对象，以便复用。


> sub()方法的使用请自行查阅，这个方法用的比较少。


## 贪婪与非贪婪


例：


```python
_import re
content = 'Hello 1234567 WorldThis is a Regex Demo'
result = re.match ('< He.__(\d+)._Demo$',content)
print(result.group(1))

```


运行结果：


```python
7

```


只得到了`7`这个数字，并没有得到我们想要的`12345678`


因为在贪婪匹配下，_`.`_会匹配尽可能多的字符。正则表达式中_`.`_后面是`\d＋`，也就是至少一个数字，并没有指定具体多少个数字，因此，_`.`_ 就尽可能匹配多的字符，这里就把`123456` 匹配了，给`\d＋`留下一个可满足条件的数字`7` ，最后得到的内容就只有数字`7`了。


为了到达想要的效果只能使用非贪婪模式，非贪婪模式匹配的写法是_`.?`_，多了个`?`就可以达到想要的效果：


```python
_import re
content = 'Hello 1234567 WorldThis is a Regex Demo'
result = re.match ('< He.__?(\d+)._Demo$',content)
print(result.group(1))

```


运行结果：


```python
7

```


## 修饰符


正则表达式可以包含一些可选标志修饰符来控制匹配的模式。修饰符被指定为一个可选的标志。


我们用实例来看一下：


```python
import re
content = _`Hello 1234567 WorldThis 
is a Regex Demo`_
`result = re.match ('< He.`_?(\d+)._Demo$',content)
print(result.group(1))

```


运行结果：


```python
AttributeError Traceback (most recent call last
AttributeError: Non eType object has no attribute group

```


运行直接报错，也就是说正则表达式没有匹配到这个字符串，返回结果为None ，而我们又调用了group ()方法导致Attribute Error 。


原因是待匹配内容中有**换行**，导致正则无法匹配，这里只需加一个修饰符`re.S` ，即可修正这个错误：


```python
result = re.match ('< He._?(\d+)._Demo$',content,re.S)

```


**常用修饰符：**


| 修饰符  | 描述                                    |
| ---- | ------------------------------------- |
| re.I | 使匹配对大小写不敏感                            |
| re.L | 做本地化识别（ locale-aware ）匹配              |
| re.M | 多行匹配，影响^和$                            |
| re.S | 使.匹配包括换行在内的所有字符                       |
| re.U | 根据Unicode 字符集解析字符。这个标志影响W 、\W 、\b 和\B |
| re.X | 该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解        |

