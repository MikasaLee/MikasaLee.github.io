---
layout: post
title: 日常Ubuntu装机
categories: blog
description: 
keywords: Ubuntu,OS,install
---

  实验室分的机子带不起 win10，所以推到重装成 Ubuntu。顺便一提尽管之前在 VMWare 上装过无数次 Linux 的各种版本，但是在真机上装除了 16/17 年学 RHCE 和 RHCA 之后好像这还是第一次。

# 0、 准备工作

+ 备份需要的数据：之前在实验室的服务器上顺手装了 apache2，然后把需要的文件都上传到apache2 的文件服务器中。
+ 制作 U盘引导盘：强烈推荐 [Ventoy](https://www.ventoy.net/en/download.html)来制作引导盘，我的移动硬盘大小总共 500G，分成两个分区：第一个分区 60G 用来做 Ventoy 需要的各种镜像；剩下的格式化之后用来存放数据。 
+ 下载 Ubuntu 镜像： [官方地址](https://ubuntu.com/download/desktop)，然后放入移动硬盘第一个分区中。（我下载的 Ubuntu 版本是：Ubuntu 20.04.1 LTS）
+ 接下来就是正常的更改BIOS引导顺序然后安装就行了，具体可以参考这篇博文：[ubuntu 20.04 LTS安装记录](https://blog.csdn.net/a805607966/article/details/105868360)，我选的是最小化安装。

# 1、 创建用户

+ Ubuntu 安装过程中我设置的用户名为：mikasalee，这也是之后用的主用户。
+ Ubuntu 上 root 在安装过程中是没有设置密码的，需要用 `sudo password root` 来设置 root 密码。
+ Ubuntu 上 root 默认是不允许图形化界面登录的，个人认识这个很好就没改。
+ 另外还新建了一个名为 default 用户用来以防万一，当 mikasalee 用户有时候崩了想去修的时候就切 tty 或者远程连 default 来修复 mikasalee

# 2、 重装 firefox

  Firefox有个全球服务和本地服务，Ubuntu下的Firefox是国际版，属于全球服务，而Windows下的Firefox是本地服务的，两个系统下默认的存储服务器不是一个，**无法同步**。
  我没有选择卸掉原来的 Firefox，而是在安装好本地服务的 Firefox 之后然后加到 PATH 中的首项中。

+ 下载 firefox：[官方下载](https://www.firefox.com.cn/)，下载的后缀为tar.gz。
+ 解压：`tar -xvf Firefox-latest-x86_64.tar.bz2`
+ 运行：进入解压后的目录然后 `./firefox` 运行
+ 添加到 PATH 中：在 Ubuntu 中，如果用户家目录下有 `bin` 目录，那么会自动将 `~/bin/` 加入到首项的 PATH 中（在 `~/.profile` 文件中 20行左右的位置），所以我直接创建 `~/bin/` 然后在其中放一个软链接：`ln -s ~/firefox目录/firefox ~/bin/firefox`
+ 记得 `source ~/.profile` 或者重启机子
+ 但是左侧 Application 栏的 firefox 标志还是系统自带的，把它 remove 了然后从下面的 show Applications 中再把本地服务的 firefox 标志给加进来。


# 3、 安装输入法

  我的 Ubuntu 版本是 20.04，个人更加推荐 google 输入法，因为安装特简单还好用。
+ 参考这篇博文：[ubuntu 20.04中文输入法安装](https://blog.csdn.net/a805607966/article/details/105874756)

# 4、 重装 VIM 

  Ubuntu 中默认是没有 vim 的，只有 vim.tiny（vim 的精简版，不能用方向键以及没有图形化界面）。卸载 vim.tiny，然后装 vim。

+ 卸载 vim.tiny：`sudo apt-get remove vim-common`
+ 安装 vim：`sudo apt-get install vim`

  另外顺便把一直用的 [.vimrc](https://github.com/MikasaLee/DataStructure/blob/master/.vimrc) 给配好。

# 5、 安装 git 并配置好与 GitHub 的连接

+ 参考 [Ubuntu下git的安装与使用](https://www.cnblogs.com/lxm20145215----/p/5905765.html)

# 6、 其他可以直接安装的

```shell
sudo apt-get install stacer # 系统管理器。这玩意真tm牛逼！！！！！！
sudo apt-get install okular # pdf阅读器
sudo apt-get install pandoc # markdown转word
sudo snap install mathpix-snipping-tool # 图片转latex
```

# 7、 安装 Typora

+ Typora 官网详细说明了如何安装 Typora：[Typora for Linux](https://typora.io/#linux)

# 8、 安装 V2ray 客户端

+ 需要借助 Qv2ray，参考：[Linux系统安装Qv2ray客户端](https://www.zsxcool.com/7137.html)

# 9、 安装百度网盘

+ 百度网盘官网也详细说明了在 Linux 上如何安装：[官网下载](http://pan.baidu.com/download)

# 10、 Python 基本环境搭建

  还是老规矩： anaconda + pycharm 。
+ anaconda 参考：[ubuntu下anaconda安装及配置](https://blog.csdn.net/a805607966/article/details/105939736)

+ pycharm 之前一直参考：[Jetbrains IDEA 2020.1年全系列版本激活教程，有效期至2089年](https://www.w3h5.com/post/530.html)，但是 2020-11-28 之后文中提到的 jetbrains-agent.jar 就不能用了（当然后面我也找到组织了并且群里面提供了最新的 jetbrains-agent.jar，但我没试过）。然后找到了 [ide-eval-resetter](https://gitee.com/pengzhile/ide-eval-resetter/)，现在在用这个，当然得等到一个月之后才知道是否有效。

# 11、 字体

  Ubuntu 上默认缺少很多 windows 上常见的字体，导致一些软件出现乱码，目前遇见到的是 WPS 和基于 deepin-wine 安装的 QQ 和微信。
  所以搜集了一波常用的字体放在 github 上了：[fonts_library](https://github.com/MikasaLee/fonts_library)

# 12、 QQ 和 微信

  这两个 TX 都没有提供合适的 Linux 版本（QQ的提供了一个北京奥运会阉割纪念版本的，微信干脆没有），需要借助 deepin-wine-ubuntu

+ [deepin-wine-ubuntu 仓库](https://github.com/wszqkzqk/deepin-wine-ubuntu)
+ 以及参考：[Ubuntu下对deepin-wine的使用详解](https://zhuanlan.zhihu.com/p/141107518)

  就能装好基于 deepin-wine 的 QQ 和 微信，但是非常有可能产生乱码问题...
  
  解决方法就是把上面搜集到的字体全部拷到 
  `.deepinwine/Deepin-QQ/drive_c/windows/Fonts/` 和 
  `.deepinwine/Deepin-WeChat/drive_c/windows/Fonts/` 目录下。
  
  然后修改 `/opt/deepinwine/tools/run_v2.sh` 文件中，进行如下修改：

```shell
#WINE_CMD="deepin-wine"
WINE_CMD="LC_ALL=zh_CN.UTF-8 deepin-wine"
```

# 13、 WPS

+ wps官方也提供了 linux 版本：[WPS Office 2019 For Linux](https://www.wps.cn/product/wpslinux#)
+ 然后 `sudu dpkg -i wpsXX.deb` 就安装完成了
+ 但是当打开 word/excel/ppt 时很可能也会提示缺少字体从而产生乱码，但是非常人性化会提醒缺少哪种字体然后还给了替换方案。不过还是建议下载这种字体而不是替换。
+ 下载好字体后拷贝到将字体拷贝到 `/usr/share/fonts/wps-office/`就OK了。