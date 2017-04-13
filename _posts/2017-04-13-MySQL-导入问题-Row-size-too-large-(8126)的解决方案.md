---
layout: post
title:  "MySQL 导入问题 Row size too large (> 8126)的解决方案"
date:   2017-04-13
categories: MySQL
tags: MySQL error
excerpt: error "Row size too large (> 8126). Changing some columns to TEXT or BLOB or using ROW_FORMAT=DYNAMIC or ROW_FORMAT=COMPRESSED may help. In current row format, BLOB prefix of 768 bytes is stored inline."
---

* content
{:toc}


> 之前在网上爬取的药品说明书，有些字段好几千字，导入数据库的时候报错了



## 解决方案


1. 将varchar改为text，问题依旧存在

2. 将**InnoDB** 改为**MyISAM**可以解决问题，但是....不是很好

3. InnoDB下的解决方案

   ```sql
   ALTER TABLE your_table
   ENGINE = InnoDB
   ROW_FORMAT = COMPRESSED
   KEY_BLOCK_SIZE = 8;
   ```

   ​

