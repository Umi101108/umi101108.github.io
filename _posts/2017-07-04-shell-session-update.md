---
layout: post
title:  "Mac SublimeText shell session update"
date:   2017-07-04 17:21:54
categories: Mac
tags: Mac error
excerpt: Mac 下sublime 运行Python文件出现错误
---


* content
{:toc}





> Sublime Text 中运行python文件总会出现“/bin/bash: shell_session_update: command not found”，虽然不影响运行，但总觉得不爽，还是得解决一下



1. get the rvm version

   ```shell
   rvm -v
   ```


2. make sure the version at least 1.26 above



3. the go ahead

   ```shell
   rvm get head
   ```


4. just work





搞定~！
