---
layout: post
title:  "Mac导入pycurl出错重装后还是报错"
date:   2018-05-01 17:23:51
categories: Error
tags: Mac Python Error
excerpt: 找了好久的pycurl库导入错误解决方案
---

* content
{:toc}


virtualenv虚拟环境下，python3安装pyspider，一直报错

```shell
ImportError: pycurl: libcurl link-time ssl backend (openssl) is different from compile-time ssl backend (none/other)
```

按照网上找到的解决方案，都是

```shell
sudo pip uninstall pycurl
export PYCURL_SSL_LIBRARY=nss 或者 export PYCURL_SSL_LIBRARY=openssl
sudo pip install pycurl 或者 sudo easy_install pycurl
```

结果还是报同样的错误

最后发现坑在了 高版本的Mac系统环境变量里是找不到openssl头文件的

所以正确的解决方案如下

```shell
pip uninstall pycurl# 卸载库
export PYCURL_SSL_LIBRARY=openssl
export LDFLAGS=-L/usr/local/opt/openssl/lib
export CPPFLAGS=-I/usr/local/opt/openssl/include# openssl相关头文件路径
pip install pycurl --compile --no-cache-dir# 重新编译安装
```

