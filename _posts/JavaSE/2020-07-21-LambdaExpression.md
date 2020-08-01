---
layout: post
title: lambda 表达式
categories: JavaSE
description: 
keywords: lambda,java
---

java8 开始支持lambda表达式。

理解 Functional Interface (函数式接口) 是学习 lambda 表达式的关键 

# 1、 函数式接口的定义
+ 任何接口，如果只包含**惟一** 一个抽象方法，那么它就是一个函数式接口。
+ 对于函数式接口，我们可以通过lambda表达式来创建该接口的对象。
+ 主要目的：避免匿名内部类定义过多。

# 2、 Demo

## 2.1、 不带参数
```java
public class TestLambda1 {
    public static void main(String[] args) {
        //原来的匿名内部类实现
        ILambda iLambda = new ILambda() {
            @Override
            public void lambdafun() {
                System.out.println("before!");
            }
        };
        //lambda实现
        iLambda = () -> {
            System.out.println("lambda!");
        };
    }
}
interface ILambda{
    void lambdafun();
}
```

## 2.2、 带参数
```java
public class TestLambda1 {
    public static void main(String[] args) {
        ILambda iLambda = new ILambda() {
            @Override
            public void lambdafun(String name) {
                System.out.println("My name is "+ name);
            }
        };
        iLambda.lambdafun("aaaaaa");
        iLambda = (name) -> {
            System.out.println("My name is "+ name);
        };
        iLambda.lambdafun("bbbbbb");
    }
}
interface ILambda{
    void lambdafun(String name);
}
```