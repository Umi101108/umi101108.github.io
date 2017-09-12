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

Jupyter Notebook 是一个交互式笔记本。Jupyter能够将实时代码、公式、可视化图标以cell的方式组织在一起，形成一个对代码友好的笔记本。



## 安装配置

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



## 快捷键

查看快捷键列表：```Help > Keyboard Shortcuts```

调出命令面板：```Ctrl + Shift + P```

进入命令模式：```Esc```

在命令模式下：

* ```A``` 在当前单元格上方插入一个新的单元格

  ```B``` 在当前单元格下方插入一个新的单元格

* ```M``` 将当前单元格更改为Markdown

  ```Y``` 将当前单元格更改为代码

* ```D + D``` 删除当前单元格

* ```Enter``` 将使你从命令模式返回到给定单元格的编辑模式

* ```Shift + Tab``` 将显示你刚刚在代码单元格键入的对象的文档

* ```Ctrl + Shift + -``` 将会将当前单元格分割为两个单元格

在代码中查找、替换、忽略输出： ```Esc + F```

在单元格和输出结果间切换：```Esc + o ```

选择多个单元格：

* ```Shift + J``` 或 ```Shift + Down``` 选择下一个单元格
* ```Shift + K``` 或 ```Shift + Up``` 选择上一个单元格
* ```Shift + M``` 合并单元格

