---
layout: post
title:  "Window下安装lxml的方法"
date:   2016-12-28 
categories: Python
tags: python error
excerpt: Windows平台安装lxml是个巨坑，整理了一个完整的解决办法
---
* content
{:toc}




> 前情提要：之前写爬虫时用bs4需要用到lxml模块，解决未果，发现html.parser可以代替之就先用着；后来学习scrapy框架发现又碰到这问题了，只能硬上了，不能怂！



## 准备工作

1. 已安装好python 
2. 已安装好pip
3. 打开cmd， 输入```pip install lxml``` 报错.....一般这种情况是常态，所以有了下文



## 解决步骤

1. 打开cmd，输入

   ``` shell
   pip install wheel
   ```

2. cmd中进入python

   ```python
   >>>import pip;
   >>>print pip.pep425tags.get_supported()
   ```

   界面输出当前python的版本信息

   ```python
   [('cp27', 'cp27m', 'win32'), ('cp27', 'none', 'win32'), ('py2', 'none', 'win32'), ('cp27', 'none', 'any'), ('cp2', 'none', 'any'), ('py27', 'none', 'any'), ('py2', 'none', 'any'), ('py26', 'none', 'any'), ('py25', 'none', 'any'), ('py24', 'none', 'any'), ('py23', 'none', 'any'), ('py22', 'none', 'any'), ('py21', 'none', 'any'), ('py20', 'none', 'any')]
   ```

   第一个元组就是我们所需要的

3. 从[这里](http://www.lfd.uci.edu/~gohlke/pythonlibs)下载对应的whl文件，根据上一步骤可知所需要的版本为*lxml-3.6.4-cp27-cp27m-win32.whl* 。（如果下错了还是无法安装的，会报平台错误）

4. 进入下载好的*.whl*文件夹，执行以下命令即可

   ```shell
   pip install lxml-3.6.4-cp27-cp27m-win32.whl
   ```

5. 搞定~~~