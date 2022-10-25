---
title: POST
date: 2020-08-07T12:48:54+08:00
tags: [CTF, Web]
categories: [CTF]
---
如题

<!--more-->

## POST是什么?

`POST`是一种`HTTP`请求方法, 通过网址中的参数来向服务器请求相应的网络资源.

更详细的`HTTP`请求方法介绍, 请见: [MDN文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)

## 如何POST?

`POST`本质上和`GET`都属于给网站提供数据的请求方法. 只不过`GET`请求可以通过在网址后面直接构造的方法来使用, 十分的简单便捷.

而`POST`不同, `POST`是一种有报文的请求方式, 正常用户是无法显式看见`POST`请求的详细信息的.

`POST`的方法有很多种, 我们只挑两种来讲.

### 使用`Postman`工具

打开`Postman`, 界面大致如下:

![Screenshot_20200807_122034.png](https://i.loli.net/2020/08/07/LOtkXhljgHNqTKd.png)

如上设置好你的数据, 就可以非常方便的向服务器发送请求了.

### 使用`Python`的`Requests`库

使用`Python`的`requests`库是最灵活也是`CTF`中最常用的方法.

#### 安装`requests`库

确保你的电脑上有`Python3`;

```bash
$ python3 -m pip install requests -i https://pypi.tuna.tsinghua.edu.cn/simple
```

**请注意! 不要将`requests`错误的写为`request`, `request`库近期被检出投毒: [链接](https://bbs.pediy.com/thread-261274.htm)**

如果你使用`Linux`, 也可以在对应发行版的软件仓库里找到`requests`库的安装包. 例如`Arch Linux`:

```bash
$ sudo pacman -Syyu python-requests
```

即可安装`requests`库.

尽量使用发行版的包管理器, 这样包管理器更新时会一并检查这些包并自动升级. 如果使用`pip`, 可能还需要隔一段时间手动更新以修复一些`bug`, 获得新的功能.

#### 使用`requests`

使用如下脚本即可简单的对一个网站进行请求:

```Python
import requests

payload = {"key" : "value"}
url = "https://www.example.com"

response = requests.post(url, data=payload) # 发送请求

print(response.text) # 输出响应主体

print(response.headers) # 输出HTTP响应头
```

`requests`库的用法十分丰富, 还可以使用`session`来维持一个会话, 请熟练掌握.

关于更高级的用法本文不再介绍, 请阅读[requests官方文档](https://requests.readthedocs.io/zh_CN/latest/)
