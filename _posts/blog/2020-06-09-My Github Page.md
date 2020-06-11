---
layout: post
title: Github Page搭建过程以及之后使用过程中遇到的问题
categories: blog
description: first blog
keywords: Github Page,jekyll,码志,gitalk,GitHub Application
---

# Github Page搭建过程以及之后使用过程中遇到的问题
   使用Github Page + jekyll 搭建博客，jekyll模板使用的是[码志](https://github.com/mzlogin/mzlogin.github.io),评论使用gitalk+GitHub Application搭建

---

## 引用
+ [使用Github pages+jekyll搭建自己的博客（windows版）](https://www.cnblogs.com/zjjDaily/p/8695978.html)
+ [码志](https://github.com/mzlogin/mzlogin.github.io)
+ [Jekyll Themes](http://jekyllthemes.org/)
+ [gitalk](https://github.com/gitalk/gitalk)
+ [基于 github issues 实现第三方评论系统](https://segmentfault.com/a/1190000011100934)
+ [GitHub Application](https://github.com/settings/applications/new)

---

## 准备
   参考[使用Github pages+jekyll搭建自己的博客（windows版）](https://www.cnblogs.com/zjjDaily/p/8695978.html) 完成Github账户注册（注：如果之后是直接fork jekyll的主题，那么无需创建Github Page，只需要fork主题之后修改仓库名称即可；如果是想自己clone/download/实现自己的jekyll模版，那么需要创建Github Page）。并在本机上安装好Ruby和jekyll。jekyll主题我选择的是[码志](https://github.com/mzlogin/mzlogin.github.io)，可以直接fork大佬的项目然后修改配置。

---

## 配置码志

### 配置_config.yml
  参考[码志](https://github.com/mzlogin/mzlogin.github.io)的fork指南部分。先在_config.yml文件中修改成自己的配置，可以参考[码志模板](https://github.com/mzlogin/mzlogin.github.io/blob/master/_config.yml)以及[我的_config.yml](https://github.com/MikasaLee/MikasaLee.github.io/blob/master/_config.yml)对照着修改。

### 配置AboutMe
   参考[码志](https://github.com/mzlogin/mzlogin.github.io)的"修改「关于」页面"。pages/about.md， _data 目录下的 skills.yml 和 social.yml 三个文件

### 配置评论模块
   我使用的是[gitalk](https://github.com/gitalk/gitalk)，参考于[基于 github issues 实现第三方评论系统](https://segmentfault.com/a/1190000011100934)。
1. 先创建一个空仓库，用来放置评论的信息。
2. 申请[GitHub Application](https://github.com/settings/applications/new)，其中Application name为空仓库的名字，Homepage URL和Authorization callback URL为你的Github Page的主页。下面是我的配置，供参考。![我的填写信息](/images/blog/InforGitHubApplication.PNG)
3.  得到GitHub Application的Client ID和Client Secret。并填入到_config.yml的配置中（132和133行）。即完成评论模块。

### 创建自己的博客
  参考[码志](https://github.com/mzlogin/mzlogin.github.io)的"删除我的文章与图片"。删掉主题的文章，**并且自己的文章命名格式为：年-月-日-文章名称**，否则码志无法扫描到此文章。

---

## 注意事项
1. 引用图片的url为: /images/xxx 。
2. 文章命名格式为：年-月-日-文章名称。






