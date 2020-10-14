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
+ [Anaconda 常用命令小结](https://zhuanlan.zhihu.com/p/57287956)
+ [ Django 官方文档](https://docs.djangoproject.com/zh-hans/3.1/)
+ [Django settings 详解](https://www.cnblogs.com/brucezhang2017/articles/8408736.html)
+ [Django—urls 系统：urls 基础](https://www.cnblogs.com/6324TV/p/8694843.html)
+ [Django_URL include (）方法](https://blog.csdn.net/qq_38375620/article/details/85562774)

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



# 2、 前端和初识 Django 

环境已经搭建完毕，接下来就是该前端模版并学习仿照 Django 模版。

## 2.1、 初识 Django 
大致理解一个 Django 服务的整体架构以及每一部分的如何关联



**启动：**

根据启动命令 `python manage.py runserver` 可知，一个 Django 应用会最先读取 manage.py文件，而这个文件中会尝试导入 Django 包以及读取和 project 同名的模块中的配置文件： settings： `os.environ.setdefault("DJANGO_SETTINGS_MODULE", "mp.settings")`

**settings 配置文件：定义此 web 服务的全局配置**

+ `ALLOWED_HOSTS` ：定义ip白名单，只有白名单中的ip才能访问。

```python
ALLOWED_HOSTS = ["*"] # *表示白名单为所有 ip 地址
```

+ `INSTALLED_APPS`：app路径，每个新建的模块都需要在这里添加

```python
 INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 以上是默认已有的，下面是自己新建/第三方包新建的
    'User',
    'captcha',
    'TestReport',
    'CheckReport',
    'TechnologyReport',
    'demo',
    'rest_framework',
    'Verification',
    'TestOutline',

]
```

+ `DATABASES`：定义连接的数据库，django 默认的数据库是 sqlite3，如果使用默认的不需要改。不过一般用的都是 mysql。

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': "mp",		#数据库名，需要提前创建
        'HOST':"127.0.0.1",
        "USER":'root',
        "PASSWORD":"redhat",
        "PORT":3306,
    }
}

```

并且需要在应用的__init__.py文件添加：

```python

import pymysql
pymysql.version_info = (1, 4, 13, "final", 0) #指定数据库的版本号
pymysql.install_as_MySQLdb() # 使用pymysql代替mysqldb连接数据库
```

+ `ROOT_URLCONF`：配置视图 view 映射层一一对应，这里只需要填写同模块下的 urls ，具体的映射过程会在 urls 文件中定义

```python
ROOT_URLCONF = 'mp.urls'
```

+ `static` 静态文件目录：css/js 以及引用的前端框架会存放在这

```python
STATIC_URL = '/static/' #调用时目录
STATICFILES_DIRS = [
    os.path.join(BASE_DIR,"static"), #具体路径
]
```
+ `TEMPLATES` 静态 HTML 目录：html 都会放在这

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],  #具体路径
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
            'libraries':{
                'staticfiles' : 'django.templatetags.static'
            }
        },
    },
]
```



+ `MIDDLEWARE` ：中间件，系统的已有的中间件以及自己写的中间件

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
     # 以上为系统已有的中间件，下面为自己写的中间件（这个 Django 模版没有自定义的中间件）
]
```

+ 定义文件上传后的存放路径：`os.path.join(BASE_DIR,"FileDir")`

```python
FileSave_DIR = os.path.join(BASE_DIR,"file") #上传文件的存放路径为项目根目录下的 file文件夹中。FileSave_DIR 为变量名，用来供程序中使用
```

+ settings 常见配置如上，如果还需要其他配置，请查阅官方文档

**urls.py：** 大白话讲，就是告诉 Django 哪个 url 去调用哪个视图代码

<font color='red'>Warning：路径问题一定要注意 '/'</font>

+ 模版中的 urls.py 文件：


```python

"""mp URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/1.11/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  url(r'^$', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  url(r'^$', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.conf.urls import url, include
    2. Add a URL to urlpatterns:  url(r'^blog/', include('blog.urls'))
"""
# 仔细阅读上面的注释，这是url编写语法。


from django.conf.urls import url,include
from django.contrib import admin
from User import views as uviews

urlpatterns = [
    url(r'^$', uviews.index), # 空url -> uviews.index，而这个 uviews 是上面import User.views时as的别名
    url(r'^admin/', admin.site.urls), # 开头为admin/ -> admin.site.urls，而这个admin 是上面import django.contrib 包中的
    url(r'^user/', include('User.urls', namespace='user')), # include()函数：表示引入其他地方的urls文件,若指定namespace，则在User.urls中必须加上: app_name = 'user' 
    url(r'^TestReport/', include('TestReport.urls',namespace="testreport")),
    url(r'^TechnologyReport/', include('TechnologyReport.urls',namespace="technologyreport")),
    url(r'^captcha/', include('captcha.urls')),
    url(r'^CheckReport/',include('CheckReport.urls',namespace="checkreport")),
    url(r'^demo/', include('demo.urls',namespace='demo_choice')),
    url(r'^Verification/',include('Verification.urls',namespace='V_report')),
    url(r'^TestOutline/',include('TestOutline.urls',namespace='testoutline')),
]

```


+ `DEBUG` ：[Django中DEBUG模式详解](https://blog.csdn.net/xujin0/article/details/83189391)


## 2.2、 更改前端模版的启动和全局配置

根据上文介绍的 Django 模版以及 .settings 文件，自定义前端模版配置的启动和 .settings 文件。

**前端模板：**
前端模版把所有 html 文件放入 templates 文件夹中，所有需要的 css 和 js 放入到 static 文件夹中，并且更改 html 的引用路径，使其能正确找到 css 和 js 文件。具体前端中哪些要改....这就不是我的工作了（逃 

PS：这模版大致写的不错，美中不足的是每个 html 文件的头文件都复制粘贴的一遍，搞的代码复用性很差，更改很麻烦，所以用了一个 `_head.html` 提取了这部分代码。

**全局配置：**

根据上文的介绍或者参考官方文档，按照自己的需求来配置全局。

# 2.3、 写后台

Django 的后台逻辑的思想也是 MVC 框架，所以基本还是按照 view -- controller -- model 的思想来布局后台代码。**但是有所不同的是先分成每个模块，然后每个模块中自己搞自己的 mvc 层。**而不像之前用 eclipse 一样先分 mvc 层，然后每层里面写所有模块各自对应的 mvc 部分。

所以下文的每个标题对应每个模块而不是mvc中的每一层。

## 2.3.1、 photo 模块

我目前接到的任务是前台需要上传一张图片放到后台服务器上就可以了。后续对图片进行深度学习的处理目前我不用管。

### 2.3.1、 创建 photo 模块

注意在 pycharm 中新创建不是直接去新建一个目录，而是有具体的命令（参考官方文档）：

```python
python manage.py startapp photo # 会在项目根目录下创建 photo 模块
```

顺便一提，如果我们不用 pycharm 来创建 Django，而是命令行创建，则是：

```python
django-admin startproject mysite # 会创建名为 mysite 的 Django 项目，然后和 pycharm 创建的相比少一个 templates 文件夹。
```


photo 模块创建完成后，目录结构为：

```python
photo/
    __init__.py
    admin.py
    apps.py 
    migrations/
        __init__.py
    models.py
    tests.py
    views.py 
```

然后一般来说还需要手动再加一个 urls.py，用来配置该模块的 URLconf。

views.py 就是用来写 view 层的代码。
models.py 就是用来写 model 层的代码。
目前来看 controlller 层不太需要，而是 view 层和 model 层的双向通信。比起 MVC 更像 MVVM 的思想。

**注意：记得在 settings.py文件中的 INSTALLED_APPS 中注册该模块**

### 2.3.2 URLconf 映射

1. 将模块的 URLconf 注册在主 URLconf 中。

```python
from photo import views as photo  # import 导包
urlpatterns = [
    url(r'^$',photo.index),				# 这是主页面的绑定
    url(r'^photo/', include('photo.urls', namespace='photo')),     # 将模块的 URLconf 注册在主 URLconf 中。注意这里末尾没有$
    url(r'^admin/', admin.site.urls),
]
```

2. 配置模块的 URLconf 。

```python
from photo import views as photo
app_name = 'photo'
urlpatterns = [
    url(r'^upload/$',photo.upload,name="upload"),
    url(r'^result/$',photo.result,name="result"),
]
```

### 2.3.3 编写该模块的 view 层代码

目前来说，需要三个视图：主页视图 index，图片上传视图 upload，图片上传后返回的结果视图 result。view.py 中的代码：

```python
# Create your views here.
from django.shortcuts import render,redirect
from Utils.tools import save_file
from myFirstDjangoProject.settings import FileSave_DIR
from photo.models import Photo

# 主页视图 index
def index(request):
    return render(request,"index.html")

# 图片上传视图 upload
def upload(request):
    return render(request,"appointment.html")           # 暂时以 appointment.html 当作图片上传模版

# 图片上传后返回的结果视图 result
def result(request):
    if request.method == "POST":
        # 得到图片
        photo = request.FILES.get('photo')    # 接收前端传过来的图片
        pname = request.POST.get('photoName') # 接收前端传过来的图片名
        uid = -1							  # 用户id，保留字段目前不用
        purl = FileSave_DIR					  # 图片保存路径
        presult = 0.00                        # 对图片处理完之后的结果，保留字段目前不用
        pinfo = "no info"                     # 对图片的相关附加信息
        if photo:
            photoModel = Photo.objects.create(photoName=pname,userId=uid,photoUrl=purl,photoResult=presult,photoInfo=pinfo)					   # model 层，向表中加入此条信息
            save_file(photo,pname,purl)       # 将图片保存到 purl 目录下，图片名为 pname。
    return redirect('/')                  # 暂时以 index.html 当作上传图片成果模版
```

对于 result 视图来说，对应的 form 表单为：
```html
<form action="" method="post" enctype="multipart/form-data">
    图片名称：<p><input type="input" name="photoName"></p>
    <p><input type="file" name="photo"></p>
    <p><input type="submit" value="submit"></p>
</form>
```


### 2.3.3 编写该模块的 model 层代码

目前来说，数据库中只需要一张 photo 表。 models.py 中的代码：

```python
from django.db import models
# Create your models here.
class Photo(models.Model):

    photoName = models.CharField(max_length=128)	# photoName 字段，char类型，最大长度为128位。下面同理
    userId = models.IntegerField(default=-1)
    photoDate = models.DateTimeField(auto_now_add=True)
    photoUrl = models.CharField(max_length=128)
    photoResult = models.FloatField()
    photoInfo = models.CharField(max_length=128)

    class Meta:
        db_table = "photo"					# 表名为：photo

    def __str__(self):
        return self.photoName
```

Django 可以根据这个 photo 类来生成创建 photo 表 ( create table )的 语句（**注意只是生成，但并没有执行，也就是说数据库中还没有这张表**）：
`python manage.py 	makemigrations photo`

这里我觉得 官方文档 已经说得很明白了：

>通过运行 makemigrations 命令，Django 会检测你对模型文件的修改（在这种情况下，你已经取得了新的），并且把修改的部分储存为一次 迁移。
>
>迁移是 Django 对于模型定义（也就是你的数据库结构）的变化的储存形式 - 它们其实也只是一些你磁盘上的文件。如果你想的话，你可以阅读一下你模型的迁移数据，它被储存在 polls/migrations/0001_initial.py 里。别担心，你不需要每次都阅读迁移文件，但是它们被设计成人类可读的形式，这是为了便于你手动调整Django的修改方式。


现在，运行 `python manage.py migrate` 命令，在数据库里创建新定义的模型的数据表。

官方文档：

>这个 migrate 命令选中所有还没有执行过的迁移（Django 通过在数据库中创建一个特殊的表 django_migrations 来跟踪执行过哪些迁移）并应用在数据库上 - 也就是将你对模型的更改同步到数据库结构上。
>
>迁移是非常强大的功能，它能让你在开发过程中持续的改变数据库结构而不需要重新删除和创建表 - 它专注于使数据库平滑升级而不会丢失数据。我们会在后面的教程中更加深入的学习这部分内容，现在，你只需要记住，改变模型需要这三步：
>
>    编辑 models.py 文件，改变模型。
>    运行 python manage.py makemigrations 为模型的改变生成迁移文件。
>   运行 python manage.py migrate 来应用数据库迁移。
>
>数据库迁移被分解成生成和应用两个命令是为了让你能够在代码控制系统上提交迁移数据并使其能在多个应用里使用；这不仅仅会让开发更加简单，也给别的开发者和生产环境中的使用带来方便。
>

你可以 cmd 中 mysql -u-p 过去检查在数据库中是否真正创建了该表


### 2.3.4 Util 工具包

至此，第一阶段的绝大部分任务已经完成，这里是搞了一个工具库，把 一些常用的工具放到这里。`Utils/tools.py` 中目前只有一个工具：将文件保存至本地（ save_file 函数）。 

```python
import os
def save_file(content,filename,save_path):
    if not os.path.exists(save_path):
        os.mkdir(save_path)
    path = os.path.join(save_path)
    if not os.path.exists(path):
        os.mkdir(path)

    _, ext = os.path.splitext(str(content))
    filepath = os.path.join(path, filename + ext)

    destination = open(filepath, 'wb+')
    for chunk in content.chunks():
        destination.write(chunk)
    destination.close()
```


# 3、 之后的工作

2020-10-14：当前工作告一段落，剩下的等前端的同学把前端页面改完然后再根据前端的接口来写 view 和 model 层。

这几天争取把 Django 官方文档从头到尾好好过一遍。