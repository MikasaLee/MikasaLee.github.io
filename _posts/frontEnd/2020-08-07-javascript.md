---
layout: post
title: javascript
categories: frontEnd
description: 
keywords: frontEnd，jQuery，javascript，json
---

复习 javascript、json、jQuery 的知识点。以及记录之后遇见这三者有趣的代码

# 1、 JavaScript

## 1.1、 js语法的一些知识点
+ 严格检查模式：第一行代码为 `'use strict'`，表示严格检查模式，预防 js 的随意性导致产生的一些问题
+ 局部变量建议使用 `let` ，而不是 `var`
+ js中的对象：{...} 表示一个对象，键值对描述属性 xxx:xxx ，多个属性之间用逗号隔开，最后一个属性不加逗号。
+ java 中的for-each遍历：js中for-in遍历下标，for-of才是for-each（es6 新特性）
+ es6 中新增 map 和 set ：

```javascript
var map = new Map([["tom",100],["tom2",1400],["tom3",1040],["tom4",1200],]);
map.get("tom");		
map.set("tom6",3434);
map.delete("tom");
var set = new Set([2,3,4,4,4,4,4,4,3,43,54]);	//只保留一个4,方法同map
```

+ 全局作用域 `window` ，js实际上只有一个全局作用域，就是window，默认情况下调用的系统变量和函数和自定义的变量和函数都在 `window` 中。eg：alert("123") 和 window.alert("123") 是等价的。
>规范：由于我们所有的全局变量都会绑定到 window 上。如果不同的js文件使用了相同的全局变量，就会产生冲突。
>所以为了解决冲突，在每一个 js 文件中我们自己显式定义自己的全局变量，然后其他所有变量和函数都放到自己的全局变量中。

eg:
```javascript
//唯一的全局变量
var all = {};
var.name = 'lrr';
var.add = function(){}
```
>jQuery 就是这么玩的，它的全局变量为jQuery，并且为了简写，等价为: $

+ es6 之后类和对象
es6 参考了java 引入了 class 和 extends 关键字，所以整个写法跟 java 很像，**但是构造器写法不一样**

```javascript
class Student{
    constructor(name) {
        this.name = name;
    }
}
var stu = new Student("aaa");

class HighStu extends Student{
    constructor(name,val) {
        super(name);
        this.val = val;
    }
}
var high = new HighStu("aaa","bbb");
```

## 1.1、 BOM
记录一下常用的 BOM 对象以及该对象表示什么，里面具体的方法和变量 console.log()或者自己查
注意所有的BOM对象都小写，首字母大写表示相应的类。
+ window （重要）：代表浏览器窗口
+ navigator（现在基本不用也不建议使用）：封装了浏览器的信息
+ screen：代表屏幕尺寸
+ location（重要）：代表当前网页的 URL 信息
+ document（最重要 DOM）：代表当前网页的 HTML 内容，DOM 文档树
+ history（不建议使用）：代表浏览器的历史记录


# 2、 JSON
> json 是什么？

早期，所有的数据传输习惯使用 XML 文件
+ JSON（javascript object notation，js对象简谱）是一种轻量级的数据交换格式
+ 简洁和清晰的**层次结构**使得JSON称为理想的数据交换语言
+ 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率

在 js 中，一切都是对象，任何js支持的类型都可以用JSON来表示；
格式：
+ 对象都用{}
+ 数组都用[]
+ 所有的键值对 都是用 key:value

JSON 和 js 对象的转换
```javascript
var user = {
    name : "lrr",
    age : 3,
    sex:'nan'
}   //  Object { name: "lrr", age: 3, sex: "nan" }

var jsonUser = JSON.stringify(user);    // {"name":"lrr","age":3,"sex":"nan"}
var user1 = JSON.parse('{"name":"lrr","age":3,"sex":"nan"}');	//JSON 本身是一串字符串!用这个字符串来表示对象
var user2 = JSON.parse(jsonUser);
```

# 3、 jQuery
>jQuery库，里面封装了大量的 js 函数

[jQuery官网](https://jquery.com/)
[jQuery API 中文文档](https://jquery.cuishifeng.cn/index.html) 多查多看

## 3.1、 初识 jQuery 
导入有两种方式：
+ 直接去官网下载jQuery文件，然后导入进你的js中
+ 通过[CDN JQuery](http://www.jq22.com/cdn/) 在线导入

jQuery 语法只有一个：`$(selector).action()`
+ selector：css选择器
+ action()：被选中标签的事件

eg：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>

</head>
<body>
<a href="" id="test-jquery">点我</a>

<script>
    $('#test-jquery').click(function () {
        alert("jQuery");
    })
</script>

</body>
</html>
```

## 3.2、  jQuery 选择器
css 的基本选择器
+ 标签选择器：`$('p').click()`
+ id选择器：`$('#id').click()`
+ 类选择器：`$('.class').click()`

还有其他各种高级选择器，[jQuery API 中文文档](https://jquery.cuishifeng.cn/index.html)  查

## 3.3、  jQuery 事件
还是参见文档，这里以鼠标事件为例的Demo

```html
mouse:<span id = "mouseMove"></span>
<div id="divMove">
    在这里移动鼠标试试
</div>
<script>
    $('#test-jquery').click(function () {
        alert("jQuery");
    })
    //当网页元素加载完毕之后，相应事件:
    // $(document).ready(function () {
    // })
    // 可以简写为：(function () {
    // })
    $(function () {
        $('#divMove').mousemove(function (e) {
            $('#mouseMove').text('x: '+e.pageX+' y: '+e.pageY);
        })
    })

</script>
```

# 4、 如何提高前端代码能力

+ 巩固js：看 jQuery 源码，看 [游戏源码](https://www.mycodes.net/166/)
+ 巩固html 和 css：扒网站，全部down下来，然后对应修改看效果