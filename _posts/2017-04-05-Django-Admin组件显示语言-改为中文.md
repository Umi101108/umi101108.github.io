---
layout: post
title:  "Django Admin组件显示语言 改为中文"
date:   2017-04-05
categories: Django
tags: Django 
excerpt: 将Django Admin 后台的界面语言改为中文
---
* content
{:toc}


因为django的admin组件默认显示为英文，自己用没问题，给客户用就不行了，所以需要翻译成中文。

还好，django做好了国际化的工作，要实现语言的转变，只需要如下操作：

在settings.py中找到MIDDLEWARE_CLASSES，

在 'django.contrib.sessions.middleware.SessionMiddleware'后面添加一个中间件 'django.middleware.locale.LocaleMiddleware'，

```python
MIDDLEWARE_CLASSES = [
    'django.middleware.security.SecurityMiddleware',
    'django.middleware.locale.LocaleMiddleware',   ## 添加这一行
    'django.middleware.common.CommonMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

启动django，就会发现admin显示为中文的了。



其他什么将 LANGUAGE_CODE='en-us' 改为 'zh_CN' 的亲测不可用



------------------------

2017-04-26 更新

发现了新的办法

django1.7之前‘en-us'是可用的，之后就改成了'zh-hans'...

```python
# settings 修改一下内容
LANGUAGE_CODE = 'zh-hans'
TIME_ZONE = 'Asia/Shanghai'
USE_TZ = False
```

