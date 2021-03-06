---
layout: post
title: python 科研民工之路
categories: python
description: 
keywords: python,machine learning,deep learning,Medical Image Reconstruction,plan
---

  科研民工计划和路，都是从了解 python3 基本语法并且只了解 python3 基本语法开始。。。

# 科研民工计划

## 0、 前言

+ 总共有三步：机器学习 -> 深度学习 -> 医学图像重建，每一步应该都包含**对应的数学知识**和 **python 常用的库/框架**。
+ 虽然是三步，但并不是走完第一步再走完第二步再走完第三步这样子，而是三个同时进行。
+ **但是要有侧重点**，比如第一阶段就是机器学习为主，另外两个为辅。其他也一样。
+ 时间有限，所以**数学知识**切记不要扎进去，点到为止，**千万千万要克制住自己的好奇心**。等到以后稳定了再慢慢想慢慢看。
+ 同理 python 常用的库/框架也是，会用为主，用的多的有疑问的去扫两眼源码就行了，**同样千万不要沉迷于源码上**。
+ 碰到某某算法，一定要**理解透并复现**，这也就是不让在前面数学知识和 python 框架上深究的原因，**研究生阶段重点是在一个个算法上**，而不是搞开发了。


## 1、 机器学习

### 1.1、 数学部分

+ 《统计学方法》pdf（李航）：ipad 上都放了半年了，该看了，但是不要指望第一次就能全懂，泛读为主

### 1.2、 python 库/框架

有篇很好的说明博客：[Numpy、Pandas、SciPy、Scikit-Learn、Matplotlib的关系以及学习资料](https://blog.csdn.net/u014410989/article/details/89947128)

#### 1.2.1、 Numpy：

+ 基于菜鸟教程中的 [NumPy 教程](https://www.runoob.com/numpy/numpy-tutorial.html)。

#### 1.2.2、 Matplotlib：

+ [Python 笔记：最全Matplotlib 入门教程](http://www.inimei.cn/archives/593.html)：前七章。
+ [NumPy  Matplotlib - 菜鸟教程](https://www.runoob.com/numpy/numpy-matplotlib.html)。

#### 1.2.3、 Pandas：

+ [ant-learn-pandas](https://github.com/peiss/ant-learn-pandas) 跑了前12讲，将 Pandas 理解成数据库就行了，所以后面表连接的操作之后就没看，用到的时候再来查。

#### 1.2.5、 scipy 和 sklearn：

+ 暂定

### 1.3、 机器学习常见的算法

**好好学好好记好好理解好好复现。**

+ [B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)：不用多bb。后续自己实现的课后作业都会放在 Github 上：[Coursera-ML-AndrewNg-myNote](https://github.com/MikasaLee/Coursera-ML-AndrewNg-myNote)
+ 《机器学习》pdf（周志华）：西瓜书，同样不用多bb，ipad上都快放的长毛了。

## 2、 深度学习

### 2.1、 数学部分

  待定

### 2.2、 python 库/框架

  tensflow 和 pytorch，具体待定

### 2.3、 深度学习常见的算法

  各种GAN，具体待定

## 3、 医学图像重建

### 3.1、 数学部分

+ [B站视频：傅里叶变换及其应用](https://www.bilibili.com/video/BV1Qx411J7ER)

### 3.2、 python 库/框架

  待定

### 3.3、 医学图像重建常见的算法

  待定



------



------



# 科研民工之路

## 2021-1-11

### 当前：

+ 目前看到[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)第 11p 结束（第二章完），[课件笔记](http://www.ai-start.com/ml2014/html/week1.html#header-n389)。

### 接下来：

+ 初步理解了线性回归以及线性回归中的梯度下降之后，应该用 Python 去复现它。不过在此之前应该先了解一下 Python 中相关的包：[孙兴华中文讲Python数据分析三部曲入门篇 Numpy教程 Pandas教程 Matplotlib教程](https://www.bilibili.com/video/BV1ji4y157uB)

## 2021-1-13

### 当前：

+ 初步了解了 Python 中的 Numpy、Matplotlib、Pandas  三个类库，不过并没有按照之前的看孙兴华的，而是找的其他教程看的：

>+ 基于菜鸟教程中的 [NumPy 教程](https://www.runoob.com/numpy/numpy-tutorial.html)。
>+ [Python 笔记：最全Matplotlib 入门教程](http://www.inimei.cn/archives/593.html)：前七章。
>+ [NumPy  Matplotlib - 菜鸟教程](https://www.runoob.com/numpy/numpy-matplotlib.html)。
>+ [ant-learn-pandas](https://github.com/peiss/ant-learn-pandas) 跑了前12讲，将 Pandas 理解成数据库就行了，所以后面表连接的操作之后就没看，用到的时候再来查。

+ 了解到还需要学习的基本类库是 scipy 和 sklearn，但是 sklearn 是将当前常见的机器学习算法封装的一个类库，而吴恩达的机器学习就是说明这些算法怎么来的。所以后续在实现每一个算法的时候要有**非sklearn的版本（自己写底层原理但只是用来理解的**和**sklearn的版本（以后实际用到的）**，所以每一个的sklearn算法等到了具体的某个算法的时候再学。
+ 另外吴恩达的视频中提到会有课后作业，所以现在也不要想着复现第二章的代码，安安静静等课后作业吧。
+ 后续自己实现的课后作业都会放在 Github 上：[Coursera-ML-AndrewNg-myNote](https://github.com/MikasaLee/Coursera-ML-AndrewNg-myNote)

### 接下来：

+ 继续往后看[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)，从12p开始（第三章）。

## 2021-1-19

### 当前：

+ 目前看到[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)，从31p结束（第五章结束）并且作业 ex1_Linear_Regression 完成。
+ 作业ex1 完成期间意识到自己的 python 及其常用类库还很不熟练，导致作业ex1写了好几天才完成，所以后续还需要多加练习。
+ 不过这也算是自己学会的第一个机器学习算法了。

### 接下来：

+ 继续往后看[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)，从32p开始（第六章）。

## 2021-1-25

### 当前：

+ 目前看到[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)，从42p结束（第七章结束）并且作业 ex2_Logistic_Regression 完成。
+ 作业 ex2 完成期间意识到自己的 python 及其常用类库比起 ex1 的时候娴熟了许多。后面再继续多练吧。

### 接下来：

+ ex2 中并没有涉及到多元分类，而是在 ex3 中，所以先打算将 ex3 中的多元分类部分完成然后再往后看新的内容

## 2021-1-26

### 当前：

+ ex3 的 part1 部分（多元分类）搞定。

### 接下来：

+ 继续往后看[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)，从43p开始（第八章）。

## 2021-1-29

### 当前：

+ 目前看到[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565) 第49p结束（第八章结束），并且ex3 剩下的 part 2部分搞定。

### 接下来：

+ 继续往后看[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)，从50p开始（第九章）。

## 2021-2-3

### 当前：

+ 目前看到[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565) 第57p结束（第九章结束），并且完成 ex4。
+ 虽然完成了作业，但是神经网络尤其是**反向传播算法**还是很模糊，以后肯定还会多次用到这玩意的，希望到时候用到的时候理解能更加深刻吧。
+ 关于相关的数学公式推导，我参考了[这个视频](https://www.bilibili.com/video/BV1QW411g718)。

### 接下来：

+ 继续往后看[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)，从58p开始（第十章）。

## 2021-2-10

### 当前：

+ 目前看到[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565) 第64p结束（第十章结束），并且完成 ex5。

### 接下来：

+ 继续往后看[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)，从65p开始（第十一章）。
+ 不过先过年，过完年再继续，过年期间使劲耍~

## 2021-2-22

### 当前：

+ 过了个年，从 2-20 开始看的书
+ 目前看到[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565) 第75p结束（第十二章结束），并且完成 ex6。
+ **但是 SVM 一点都没理解，每一小节都听得很懵逼，并且练习题并没有让自己仿写一个SVM而是直接用的类库，所以也没有加深理解。SVM以后还得找其他资料再重新看重新学。**


### 接下来：

+ 继续暂停，要准备期末考试了。考完再回来接着看。

## 2021-3-14

### 当前：

+ 考完了试，从3-7 开始看的书
+ 目前看到[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565) 第87p结束（第十四章结束），并且完成 ex7。
+ K-means 和 PCA 理解的还算可以

### 接下来：

+ 继续往后看[B站吴恩达](https://www.bilibili.com/video/BV164411b7dx?from=search&seid=7481822073233109565)，从88p开始（第十五章）。
