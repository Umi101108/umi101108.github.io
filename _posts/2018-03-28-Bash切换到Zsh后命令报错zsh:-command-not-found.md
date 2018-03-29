---
layout: post
title:  "Bash切换到Zsh后命令报错zsh: command not found"
date:   2018-03-28 16:09:55
categories: Linux
tags: Linux Zsh
excerpt: 整理了一份数据库设计规范！
---

* content
{:toc}




## 问题描述

把Linux默认的Bash替换为了Zsh，然后使用各种命令时老是提示

```
zsh: command not found: XXX
```



## 解决方案

1. bash中添加路径

   ```bash
   vim ~/.bash_profile
   # 增加以下内容
   export PATH=/bin:/usr/bin:/usr/local/bin:$PATH
   ```

2. 执行脚本

   ```bash
   source .bash_profile
   ```

3. 一劳永逸

   ```bash
   # 在~/.zshrc中添加以下内容
   source ~/.bash_profile
   ```

   ​

