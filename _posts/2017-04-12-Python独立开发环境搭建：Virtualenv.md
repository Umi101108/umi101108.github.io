---
layout: post
title:  "Python独立开发环境搭建：Virtualenv"
date:   2017-04-12
categories: Python
tags: Python virtualenv 环境 
excerpt: 用virtualenv创建Python独立开发环境
---

* content
{:toc}




鉴于Python第三方的包众多，不同版本之间可能会有差异。保险起见，为每个应用创建一个隔离的Python运行环境显得尤为重要，就用第三方库**virtualenv** 来解决这个问题吧



## Windows环境下

没办法，公司还是用的Windows，心塞...

#### 安装

```shell
pip install virtualenv
```

#### 创建项目

```shell
mkdir myproject
cd myproject
```

#### 创建一个独立的Python开发环境

```shell
virtualenv --no-site-packages my_env   ## 不带任何第三方包的纯净的运行环境
```

#### 激活环境

```shell
env\Scripts\activate
```

#### 查看当前库

```shell
pip list
```

#### 显示所有依赖

```shell
pip freeze
pip freeze > requirements.txt	## 生成requirements.txt文件
pip install -r requirements.txt		## 根据requirements.txt文件生成相同的环境
```

#### 关闭virtualenv

```shell
deactivate
```





## Mac环境下

操作过程中又发现了一个虚拟环境管理包：**virtualenvwrapper**

```shell
$ pip install virtualenvwrapper
$ export WORKON_HOME=~/Envs
$ mkdir -p $WORKON_HOME
$ source /usr/local/bin/virtualenvwrapper.sh
$ mkvirtualenv envl
```

安装完成后会自动进入新的环境。

这时，因为安装过anaconda的关系，可能会出现OSError。此时，需要用默认python来创建虚拟环境

```shell
$ mkvirtualenv test -p /usr/bin/python 
```

