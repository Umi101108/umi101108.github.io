---
layout: post
title:  "python使用requests中文乱码问题"
date:   2017-01-06 
categories: Python
tags: python error
excerpt: python使用requests模块爬取网页，调取text方法时中文显示乱码问题
---
* content
{:toc}


> 先写点废话......新的一年开始了，躺尸几天才重新开始写....要努力啊少年



今天爬取[**用药参考**](http://drugs.medlive.cn/drugref/html/11226.shtml) 这个网站时

```python
import requests
response = requests.get(url)
print response.text
```

显示的html文档里中文全变成了乱码....囧

加一条代码查看页面编码

```python
print response.encoding
```

显示为

> ISO-8859-1

好家伙，那就修改下编码吧

```python
import requests
response = requests.get(url)
response.encoding = 'utf-8'
print response.text
```

再运行，搞定~~~