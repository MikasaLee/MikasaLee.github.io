---
layout: post
title: 记录学习 Django 官方文档之路
categories: python
description: 
keywords: python,web,Django
---

记录下学习 Django 官方文档中遇到的问题

# 0、 准备

## 0.1、 参考文档

+ [Django 官方文档地址](https://docs.djangoproject.com/zh-hans/3.1/)

## 0.2、 搭建环境

官方给的环境是在 linux 上从 0 开始，python + pip 一步一步搭起来。但我不想这么麻烦，借助 anaconda 更加便利。所以需要准备的环境为：

+ linux （普通用户，不允许用 root ），实验室是 Ubuntu 
+ anaconda3 
+ mysql 8.0 

然后 anaconda3 新建一个环境 DjangoEnv。DjangoEnv 中的环境为：

+ python 3.6.12 
+ pip 20.2.3
+ django 3.1.2

当然是其他 python 版本。只要注意 pip 、 django 、python 三者兼容即可

## 0.3、 给服务器加映射

由于我用的 linux 是实验室的服务器 Ubuntu 而不是本地主机的虚拟机或者云服务，并且这是两个局域网，所以我需要加一个 NAT 映射。就和 ssh 一样。

这里我加了两条：

101.7.171.8:56666  ——>  192.168.1.101:80 （毕竟是 web 服务，还是尊重下默认端口）
101.7.171.8:56667  ——>  192.168.1.101:8000 （ Django 默认端口是 8000）


## 0.4、 测试

环境搭建好后并且建好官网给的 Demo（名为 mysite） 顺便再 apt 安装一下apache2。 

**注意： 启动 mysite时命令为：**

```python
python manager.py runserver 0.0.0.0:8000 # 表示对所有源ip开放 8000 端口。
```

启动 mysite 和 apache2。

最后浏览器访问 101.7.171.8:56666 以及 101.7.171.8:56667
