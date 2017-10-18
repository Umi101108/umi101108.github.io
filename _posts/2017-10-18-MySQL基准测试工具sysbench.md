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

   ​