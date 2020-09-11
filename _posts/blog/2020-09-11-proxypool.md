---
layout: post
title: 搭建proxy pool
categories: blog
description: 
keywords: v2ray,vpn,ssr,proxypool
---

借助大佬@zu1k写的开源工具来搭建 proxypool 。自动抓取tg频道、订阅地址、公开互联网上的ss、ssr、vmess节点信息，聚合去重测试可用性后提供节点列表

# 0、 前言
+ 作者的 github 项目地址 ：[ zu1k/proxypool ](https://github.com/zu1k/proxypool)
+ 原项目是用 golang 写的，但是没学过 golang 的根据此文章也能完成（因为我也没学过...）。
+ 在原项目 README.md 文件中作者已经大致说明安装过程。只是本人太弱碰见了很多问题，所以才有这篇博客来记录自己的安装过程。
+ 我是只用 vmess ，所以ss、ssr、clash等其他工具不保证通过此博文能成功。虽然理论上没问题
+ **严禁用作商业用途，严禁通过此工具获取报酬。希望大家可以自觉遵守**
+ 截止于2020-09-11，作者的该项目已清空。如下图：
 ![zu1k/proxypool](/images/blog/proxypool1.PNG)

# 1、 准备环境
因为我是自己捣鼓摸索了一天，其中只要是用到环境不管有没有用都会列在这。
+ 一台虚拟机centos7
+ xftp、xshell

# 2、 安装 proxypool
这步作者说的很明白了:
>首先 Fork 仓库到你的用户名下，然后修改 assets 文件夹下的html文件，将里面的域名修改为你自己的，最后点击按钮进行部署

按照作者说的做。

