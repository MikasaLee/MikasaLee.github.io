---
layout: post
title: 第一次Django实战
categories: python
description: 
keywords: python,web,Django
---

实验室需要做一个后台用 Python 的 web 网站。而 Django 是 Python 中主流优秀的 Web 框架之一。Django 我从来没学过，学长给了一个纯前端的模版，然后跟同学要了一个 Django 项目的模版。根据这个 Django 项目把后台实现并且和前端结合起来

# 0、 参考
+ [anaconda 官方文档](https://docs.anaconda.com/anaconda/user-guide/getting-started/)
+ [Anaconda 与 conda 区别](https://blog.csdn.net/wangyuankl123/article/details/103011550)
+ [Anaconda常用命令小结](https://zhuanlan.zhihu.com/p/57287956)

# 1、 部署环境

## 1.1、 前端模版

这个简单，创建 Django 项目然后把这些文件都拷贝到 `templates` 文件夹下。然后会遇到 `css` 等引入的其他文件路径不对导致页面展示和原本的不一样，不过这些目前不管，只需要主页正常访问到就行了

PS：Django 正确启动方法为：
+ 在  `Terminal` 输入 ：`python manage.py runserver`
+ 然后正常启动后会得到服务器的 socket ，默认为：`127.0.0.1:8000`，然后浏览器访问
+ 如果能正确显示 index.html 页面（不包括 index.html 文件中包含的其他文件），则说明服务器正常访问

## 1.2、 Django 模版

这个就比较麻烦了，这个 Django 模版虽然也是 pycharm 项目，直接导入即可，但是因为 anaconda 环境不一样，所以很多依赖包找不到，需要自己一个一个加进去。

加进去的方法为：按照 Django 正确启动方法启动会报错，一般就是说缺少啥包，然后在 `Terminal` 中使用 anaconda 或者 pip 导入即可。推荐先使用anaconda导入，anaconda找不到的包再使用 pip 导入，具体导入步骤参见下文 1.3 anaconda 常用命令 。并且该模板使用的数据库为 MySql 8.0 所以还需要去官网下载并且装好 MySql

依赖包都导入之后，将模版中的连接数据库的用户密码和数据库名更改，并在 MySql 中创建该数据库。**注意只创建数据库，而不创建表**

在 `Terminal` 中使用 `python manage.py migrate` 可以**动态的将 model 映射为数据库中的表**

然后 `python manage.py runserver` 启动服务测一测能不能正常运行。

## 1.3、 anaconda 常用命令

### 1.3.0、 anaconda 和 conda
+ 参考： [Anaconda 与 conda 区别](https://blog.csdn.net/wangyuankl123/article/details/103011550)

**注意这是两个不同的东西，所以输入命令的时候需要注意第一个是 conda 还是 anaconda，一般来说涉及到第三方类库的时候 conda解决不了，需要通过 anaconda 解决；如果 anaconda也解决不了，那么只能 pip/或者手动导入了**

### 1.3.1、 管理 conda

+ 检查版本： `conda --version`
+ 升级版本： `conda update conda`
+ 查看包括版本的更多信息：`conda info`
+ 查看帮助信息：`conda -h`  多看帮助文档

### 1.3.1、 管理环境

环境是 anaconda 中用来管理所有下载的包。

+ 创建相关的帮助信息：`conda create --help ` 需要相关的参数去查文档，比如指定该环境的python版本
+ 创建环境：`conda create --name envname` 创建一个名为 `envname` 的环境
+ 显示环境：`conda info -e`
+ 激活/更改环境：`activate envname` 激活/更改到 `envname` 环境中。 **如果你处于某一环境中，那么命令行的路径提示前会加上该环境。** 
如： `(base) D:\PythonProject\>`  表示当前位于 `base` 环境中

### 1.3.2、 管理包

这部分是 anaconda 中使用频率最高的。

+ 查看当前环境安装的包：`conda list`
+ 模糊查找（只要包含字符串就显示）： `conda search packagename` 模糊查找包名为 `packagename` 的包
+ 安装包：`conda install packagename`  安装报名为 `packagename` 的包


### 1.3.3、 安装第三方的包
对于第三方类库的包，conda install 直接安装是不行的。比如 `conda install djangorestframework` ，会报
```python
PackagesNotFoundError: The following packages are not available from current channels:

  - djangorestframework
```

这时就需要 anaconda 而不是 conda 了。
+ `anaconda search -t conda djangorestframework` 可以列出来所有 anaconda 平台搜集的包，然后找见自己需要的包：比如`djangorestframework` 需要的是 `conda-forge/djangorestframework |   3.12.1 | conda           | linux-64, win-32, noarch, osx-64, win-64 | py_0, py36_0, py34_0, py35_0, py27_0`

+ 然后 `anaconda show conda-forge/djangorestframework` 会得到安装此包的命令
+ ` conda install --channel https://conda.anaconda.org/conda-forge djangorestframework` 安装即可

对于有些包 anaconda 平台上没有，那么只能通过 pip/手动导入了

###  1.3.4、 分享环境

如果你想把你当前的环境配置与别人分享，这样ta可以快速建立一个与你一模一样的环境（同一个版本的python及各种包）来共同开发/进行新的实验。一个分享环境的快速方法就是给ta一个你的环境的.yml文件。

首先通过 `activate target_env` 要分享的环境 `target_env` ，然后输入下面的命令会在当前工作目录下生成一个 `environment.yml` 文件

```conda env export > environment.yml```

小伙伴拿到environment.yml文件后，将该文件放在工作目录下，可以通过以下命令从该文件创建环境

```conda env create -f environment.yml```


# 2、 写后台改模版