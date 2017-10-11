---
layout: post
title:  "python使用requests时遇到SSLError的解决方案"
date:   2017-10-11
categories: Python
tags: error Python
excerpt: python使用requests时遇到SSLError的解决方案，因为碰到两次这个问题，所以还是得记下来才行
---

* content
{:toc}


第一次是爬取贴吧时，在使用requests时报错SSLError，

通过禁用校验参数可以解决

```python
requests.get(url, verify=False)
```

第二次是使用itchat登录微信时，又报错了SSLError

```shell
SSLError: HTTPSConnectionPool(host='https://login.weixin.qq.com', port=443): 
 (Caused by SSLError(SSLError(8, '_ssl.c:507: EOF occurred in violation of protocol'),))
```

因为已经封装好了，所以通过第一种方法不太方便，于是有了下面的方法

```shell
pip uninstall -y certifi
pip install certifi
```

通过重新安装certifi这个包就能解决，一劳永逸

出现这个原因应该是云服务器上的证书过时了，重新安装最新版即可解决