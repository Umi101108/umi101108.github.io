---
layout: post
title:  "Pycharm报错"
date:   2017-05-11
categories: Python
tags: Pycharm Python
excerpt: Pycharm
---

* content
{:toc}




用virtualenvwrapper创建好虚拟环境，在命令行中运行成功，但是在Pycharm中运行失败，提示Python mysqldb: Library not loaded: libmysqlclient.18.dylib reason: image not found。

Pycharm中intrepeter配置没问题，试了好多遍最后的解决方案如下

在终端中运行

```shell
sudo ln -s /usr/local/mysql/lib/libmysqlclient.18.dylib /usr/lib/libmysqlclient.18.dylib
sudo ln -s /usr/local/mysql/lib /usr/local/mysql/lib/mysql
```



但是在运行时又会出现一个新的问题 **Operation not permitted**

原来在新的OSX中，有个Rootless机制，限制了直接写入系统文件。所以还需要先关闭这个机制

1. 关闭Rootless机制，重启按住Command+R键，进入恢复模式，打开终端执行

   ```shell
   csrutil disable
   ```

   重新启动，进入系统执行你要授权的操作

2. 恢复Rootless机制，依然需要重启进入恢复模式，在终端中执行

   ```shell
   csrutil enable
   ```

   ​



