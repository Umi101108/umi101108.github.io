---
layout: post
title:  "MySQL基准测试工具sysbench"
date:   2017-10-18
categories: MySQL
tags: MySQL 测试 sysbench
excerpt: sysbench基准测试工具安装及使用说明
---

* content
{:toc}
> 根据网上的sysbench安装教程踩了很多坑，一直装不上，千辛万苦终于找到了可以正确安装的方法



## Sysbench



### 安装

1. 下载解压

   ```shell
   wget https://github.com/akopytov/sysbench/archive/1.0.zip -O "sysbench-1.0.zip"
   unzip sysbench-1.0.zip  #如果没安装zip则yum install unzip zip
   cd sysbench-1.0
   ```

2. 安装依赖

   ```shell
   yum install automake libtool –y
   ```

3. 安装

   ```shell
   ./autogen.sh
   ./configure
   export LD_LIBRARY_PATH=/usr/include/mysql
   make
   make install
   ```

4. 安装成功

   ```shell
   sysbench --version
   ```

   ————make时遇到问题的解决方案————

   如果在make时遇到问题

   ```shell
   /usr/bin/ld: cannot find -lmysqlclient_r
   collect2: error: ld returned 1 exit status
   ```

   cannot find -l后面跟的是库文件，就是mysqlclient_r库文件找不到

   去根目录找一下

   ```shell
   find / -name "*mysqlclient_r*"
   /usr/lib64/mysql/libmysqlclient_r.so.18
   /usr/lib64/mysql/libmysqlclient_r.so.18.1.0
   ```

   可以看到库文件是有的，只是带了个数字后缀，给库文件建立软链接即可

   ```shell
   ln -s /usr/lib64/mysql/libmysqlclient_r.so.18 /usr/lib64/mysql/libmysqlclient_r.so
   ```

   重新make即可