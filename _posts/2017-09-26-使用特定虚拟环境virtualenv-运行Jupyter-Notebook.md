---
layout: post
title:  "使用特定虚拟环境virtualenv 运行Jupyter Notebook"
date:   2017-09-26
categories: Linux
tags: Linux Python Jupyter 配置
excerpt: Jupyter Notebook 中配置kernel实现 可以选择在特定virtualenv下运行notebook
---

* content
{:toc}


## 使用特定虚拟环境运行Jupyter Notebook 

因为Jupyter Notebook默认kernel是机子本身的Python，而python脚本往往在特定虚拟环境下运行，所以需要进行相应的配置使在Notebook中能使用特定的virtualenv。



```shell
# 建立虚拟环境scrapy
mkvirtualenv scrapy
# 进入虚拟环境scrapy
workon scrapy
# 安装jupyter，具体操作略过
(scrapy) pip install jupyter
# 安装ipykernel，添加kernel
(scrapy) pip install ipykernel
(scrapy) python -m ipykernel install --user --name scrapy --display-name "Python2(scrapy)"
```

之后重启jupyter notebook 就可以了。



在Jupyter Notebook面板```Kernel >> Change Kernel >> <list of kernels>``` 你就能看到刚刚添加的内核```Python2(scrapy)``` 了。

附上详细文档https://github.com/ipython/ipython/blob/7c12b021ee7bdcaf8cec814a624203d8e74aab08/docs/source/install/kernel_install.rst#kernels-for-different-environments