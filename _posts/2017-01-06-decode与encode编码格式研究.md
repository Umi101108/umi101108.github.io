---
layout: post
title:  "decode与encode编码格式研究"
date:   2017-01-06 
categories: Python
tags: python error
excerpt: python中碰到各式各样的乱码问题，通过decode和encode来解决，那么怎区分呢
---
* content
{:toc}


>python中一碰到中文难免会碰到各种编码问题，各种显示乱码，报错呀啥的。decode和encode可以解决这个问题，那么这两者有什么区别呢



### python内部编码

字符串在Python内部编码为**unicode**编码，所以在做编码转换时需要以unicode为中间编码来进行转换，即需要先将其他编码解码（decode）成unicode，在从unicode编码（encode）为另一种编码



### 如何获得当前编码

需要转换就需要先知道当前编码是什么

* 查看系统编码

  ```python
  import sys
  print sys.getdefaultencoding() ## 获得系统默认编码
  ```

* 查看文本编码

  ```python
  str = 'hahaha'
  str2 = u'哈哈哈'
  print str.__class__
  print str2.__class__
  ```

* 判断是否为unicode编码

  ```python
  isinstance(s, unicode) ## 若为True，则是unicode编码
  ```



### 解码 decode

在有些**gb2312** 编码的文件中，显示可能会乱码，需要进行编码转换。

```python
str.decode('gb2312')  ## 将gb2312编码的字符串解码为unicode编码
```



### 编码 encode

为了正确输出，需要对unicode编码的字符串进行编码

```python
str.encode('utf-8')  ## 将unicode编码的字符串编码为utf8编码
```





### 万能模板

```python
s = "中文"

if isinstance(s, unicode):
    ## s=u"中文"
    print s.encode('gb2312')
else:
    ## s="中文"
    print s.decode('utf8').encode('gb2312')
```

需要其他编码转换自行替换代码~~



### \u字符串转换为unicode类型

```python
s = '\u5317\u4eac'
print s.decode('raw_unicode_escape')
```



### 列表中包含中文的输出

```python
# 列表中包含中文，循环输出可以显示正常，但是直接打印列表还是会乱码
import json
list = ['中文', '英文']
print json.dumps(list, ensure_ascii=False, indent=2)
```

