---
layout: post
title:  "Linux服务器端配置Python交互式开发环境Jupyter Notebook"
date:   2017-07-28
categories: Linux
tags: Linux Python Jupyter 配置
excerpt: Linux服务器端配置Python交互式开发环境Jupyter Notebook，实现外网登录服务器即可运行python脚本的功能
---

* content
{:toc}


# Jupyter Notebook

Jupyter Notebook 是一个交互式笔记本。



1. 安装Jupyter notebook

   ```shell
   pip install jupyter notebook
   ```

2. 启动notebook

   ```shell
   jupyter notebook
   ```

   这是会报错，不建议使用root账号运行启动命令

   ```shell
   Running as root is not recommended. Use --allow-root t.
   ```

   **解决方法**

   * 修改配置文件，执行```jupyter notebook --generate-config``` 即可初始化配置文件，需要加入```--allow-root``` 才行。

     ```shell
     [root@localhost ~]# jupyter notebook --generate-config --allow-root
     Writing default config to: /root/.jupyter/jupyter_notebook_config.py
     ```

   * 创建一个密码，这样以后就不用每次都复制URL地址了

     ```shell
     ipython
     In[1]: from notebook.auth import passwd
     In[2]: passwd()
     Enter password:
     Verify password:
     Out[2]: 'sha1:f6ac161e9215:dc0eb8c8d43b74e32bb03db161e3261ea5d7c927'
     ```

   * 修改配置文件中的IP地址、工作目录、并添加认证密码

     ```shell
     vim /root/.jupyter/jupyter_notebook_config.py
     ```

     ```shell
     # jupyter_notebook_config.py

     #c.NotebookApp.allow_root = False
     去掉注释，并修改为True即可解决root权限运行的问题

     #c.NotebookApp.ip = 'localhost'
     去掉注释，并把localhost改成0.0.0.0，这样就可以在外部进行访问了

     #c.NotebookApp.notebook_dir = u''
     去掉注释，并修改为u'/opt/jupyter'，这样就默认会把notebook上创建的文件保存到该目录下，当然目录需要事先创建好

     #c.NotebookApp.password = u''
     去掉注释，并加入之前创建好的密码sha1
     ```

3. 进入notebook界面

   ```shell
   jupyter-notebook
   ```

   在浏览器中输入```http://ip:8888```

   Bingo~