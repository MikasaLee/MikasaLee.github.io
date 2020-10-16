---
layout: post
title: linux中非root安装mysql
categories: database
description: 
keywords: mysql, linux, install
---

记录一次在实验室的服务器中在自己的账号下（非 root ）安装 mysql ，然后网上找见一篇特别牛逼的博客，大佬把所有能遇见的意外情况都记录下来了。美中不足的是大佬装的是 mysql5.7 的版本，我的需求是 8.0 的版本，然后还要加入到 PATH 中，所以会在大佬的基础略微改动。

# 1、 跟着大佬走
如果想安装 mysql5.X的版本，那么请完全跟着大佬走不用在往下看了：[Linux普通用户安装mysql-5.7.21](https://blog.csdn.net/tianpy5/article/details/79842888)



# 2、 自己的一些修改



## 2.1、 安装过程

因为是安装 mysql 不同的版本，所以会有细微的差别

**修改 root 密码：**

不同版本登录进 mysql 后修改 root 密码的命令为：
```mysql
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456'); # mysql5.X 修改密码 
SET PASSWORD FOR root@localhost = '123456'; # mysql 8.X 修改密码
```

**libaio 包的问题：**

[mysql 官方文档说了这个问题](https://dev.mysql.com/doc/refman/5.6/en/binary-installation.html)：因为实验室是 ubuntu 而不是 centos ，所以基于 apt-get 的命令为：

```shell
shell> apt-cache search libaio # search for info
shell> apt-get install libaio1 # install library，注意有个1
```

## 2.2、 简化命令

**mysql 启动：**

在大佬博客中，写到 mysql 的启动方式为：

```shell
[2iuser@iZuf637wclk4jkwphn8rjyZ mysql]$ bin/mysqld_safe \
--defaults-file=/home/2iuser/mysql/my.cnf \
--user=2iuser &
[1] 21651
[2iuser@iZuf637wclk4jkwphn8rjyZ mysql]$
```

在 Unix 机器上，MySQL 官方推荐使用 `mysqld_safe` 启动 `mysqld` 服务。`mysqld_safe` 增加了一些安全特性，比如它会在遇到错误时重启 `mysqld` 服务，并把运行时信息记录到 error log 中。

但是每次输这个命令太长了，可以自己写个小脚本来简化这个命令：

```bash
#!/bin/sh
# 自定义 mysql 启动脚本
mysqld_safe --defaults-file=/home/lirunrui/mysql/my.cnf --user=lirunrui &   
```

至于脚本的名字有两种思路： 

一种是将原来的 `mysqld_safe` 改个别的名字，然后这个脚本名为 `mysqld_safe`，使用时依旧是 `mysqld_safe`，这样的话要记得脚本中就不是 `mysqld_safe`而是新名字。

一种是脚本为新名字，然后使用的时候用这个新名字。我这里采用的就是第二种，新名字为  `mysqlstart`。。。（实在想不到好名字了>__<）

同理，**mysql 关闭：**

```bash
#!/bin/sh
# 自定义 mysql 关闭脚本
mysqladmin shutdown -u root -p -S /home/lirunrui/mysql/mysql.sock 
```
名字为： `mysqlshutdown`

**连接 mysql：**

```bash
#!/bin/sh
# 自定义连接 mysql 脚本 
mysql -u root -p -S /home/lirunrui/mysql/mysql.sock  
```
名字为： `mysqlconnect`

## 2.3、 PATH 环境变量

将 mysql 加入到环境变量中。

```bash
# .bashrc 最后加这句话。
# 因为我上面新建的三个脚本也放在 ~/mysql/bin 目录中了，所以不需要另外加 PATH 去找他们了。
export PATH=/home/lirunrui/mysql/bin:$PATH 
```