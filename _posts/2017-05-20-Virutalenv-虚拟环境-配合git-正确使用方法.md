---
layout: post
title:  "Virutalenv 虚拟环境 配合git 正确使用方法"
date:   2017-06-08 
categories: Linux
tags: Linux
excerpt: 在不同机子的虚拟环境中同步依赖包
---
* content
{:toc}


## Virutalenv 虚拟环境 配合git 正确使用方法

直接将虚拟环境env1的文件全部复制到env2里，然后修改涉及路径的文件，这种想法可能可行，但是风险太大，显然不是个好方法。正确方法如下

1. 进入原虚拟环境env1，执行

   ```shell
   pip freeze > requirements.txt
   ```

   将包依赖信息保存在requirements.txt文件中

2. 进入虚拟环境env2，执行

   ```shell
   pip install -r requirements.txt
   ```

   pip会自动下载并安装所有包