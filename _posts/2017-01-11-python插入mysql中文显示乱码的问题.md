---
layout: post
title:  "python插入mysql中文显示乱码的问题"
date:   2017-01-11 
categories: Python 
tags: python error 乱码
excerpt: python插入mysql时中文显示乱码的解决方法
---
* content
{:toc}
> 前情提要：python爬取的数据想要存入数据库中，发现存进去的中文都是乱码...



分两步走：mysql配置，python配置（这不是废话吗）



## mysql 配置

mysql的默认编码是**Latin1**，不支持中文，应该设置为**utf8**。

```mysql
## 查看设置
show variables like '%char%'
```

若Value值为utf8则设置正确，否则，则需修改编码

```mysql
## 修改编码
SET character_set_client='utf8';
SET character_set_connection='utf8';
SET character_set_results='utf8';
```

至此，在mysql命令行中就可以插入和显示中文了。但是python连数据库插入的还是乱码啊啊！！

——更新——————

新建数据表时，也要注意设定的字符集

```sql
CREATE TABLE `dd_chaptername` (   
  ···
) ENGINE=InnoDB DEFAULT CHARSET= utf8mb4;
```

查看数据表每列的字符集

```sql
show full columns from table_names;
```



## python 配置

首先当然要申明使用**utf8**编码

```python
# python文件设置编码utf-8
# -*- coding:utf-8 -*-
```

配置好数据库连接选项

```python
import MySQLdb
conn = MySQLdb.connect(
	host = 'localhost',
	port = 3306,
	user = 'root',
	passwd = 'gt123456',
	db = 'directions'
	)

cur = conn.cursor()
```

然后

```python
cur.execute("set NAMES utf8")  #一般到这一步就行了
cur.execute('SET CHARACTER SET utf8;')
cur.execute('SET character_set_connection=utf8;')
```

就可以愉快的执行插入中文的命令了

——如果还不行的话----

```python
conn.set_character_set('utf8')
```





### 备选方案

这个方案对我没有用，不过可以试试

```python
# 修改python默认编码
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
```

