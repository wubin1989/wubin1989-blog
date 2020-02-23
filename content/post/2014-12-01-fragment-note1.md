---
title:       "碎片笔记(一)"
subtitle:    ""
description: ""
date:        2014-12-01
author:      "wubin1989"
image:       ""
tags:        ["frontend"]
categories:  ["Tech" ]
archives:    "2014"
---

## 用“==”进行比较，用“=”来赋值
## IE10浏览器的前缀“-ms-”
## 运算符
算术运算符中的单目运算符包括：-（取反）、~（取补）、++（递加1）、--（递减1），双目运算符则包括：+（加）、-（减）、*（乘）、/（除）、%（取模）、|（按位或）、&（按位与）、<<（左移）、>>（右移）、>>>（右移，零填充）。  

比较运算符的基本操作过程是，首先对它的操作数进行比较，之后返回一个true或false值，有8个比较运算符：<（小于）、>（大于）、<=（小于等于）、>=（大于等于）、= =（等于）、!=（不等于）。

Javascript中增加了几个布尔逻辑运算符，包括：

!（取反）、&=（与之后赋值）、&（逻辑与）、|=（或之后赋值）、|（逻辑或）、^=（异或之后赋值）、^（逻辑异或）、?:（三目操作符）、||（或）、= =（等于）、|=（不等于）。

## 穿透点击
pointer-events: css3属性，穿透点击。
绝对定位元素盖住链接或添加某事件handle的元素后，那么该链接的默认行为（页面跳转）或元素事件将不会被触发。
现在Firefox3.6+/Safari4+/Chrome支持一个称为pointer-events的css属性。使用该属性可以决定是否能穿透绝对定位元素去触发下面元素的某些行为。如下
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>CSS:pointer-events</title>
<style type="text/css">
    .overlay1 {
        width:80px;
        height:20px;
        background:gold;
        position:absolute;
        top:5px;
        left:5px;
        opacity:0.5;
    }
    .overlay2 {
        width:80px;
        height:20px;
        background:gold;
        position:absolute;
        top:40px;
        left:5px;
        opacity:0.5;
    }
    .pointer{pointer-events:none;}
</style>
<script type="text/javascript">
window.onload = function(){
    document.getElementById('chx').onclick = function(){
        document.getElementById('a').className
            = "overlay1 " + ((this.checked)? "pointer" : "");
        document.getElementById('b').className
            = "overlay2 " + ((this.checked)? "pointer" : "");
    }
}
</script>
</head>
<body>
    <div id="a" class="overlay1"></div>
    <div id="b" class="overlay2"></div>
    <a href="http://mail.sina.com.cn">SinaMail</a>
    <br/><br/>
    <span onclick="alert(3);">SinaMail</span>
    <p>
        <input id="chx" type="checkbox">
        <label for="chx">开启穿透点击</label>
    </p>
</body>
</html>
```

## 框架
框架就是一个你可以用于你的网站项目的基本
的概念上的结构体。css框架通常只是一些css文件
的集合，这些文件包括基本布局、表单样式、网格
或简单结构、以及样式重置。比如：
typography.css 基本排版规则
grid.css 基于网格的布局
layout.css 通常的布局
form.css for 表单样式
general.css 更多通用规则

CSS 框架是一系列 CSS 文件的集合体，包含了基
本的元素重置，页面排版、网格布局、表单样式、
通用规则等代码块,用于简化web前端开发的工作，
提高工作效率

## 视图
UIkit 包含一系列为各种不同的视图宽度实现响应式内容的类。下面的表格提供了一个关于一些可用的视图断点以及相应的设备概述。你可以通过 定制器 来调整所有的断点。
大小视图设备Mini小于479px手机纵向Small480px 到 767px手机横向Medium768px 到 959px平板电脑纵向Large960px 到 1199px台式机或平板电脑横向Xlarge1200px 或以上大屏幕设备

## 规范中注明 margin 的百分比值参照其包含块的宽度进行计算。

## 网页标题图标
```
<link href="/gl/favicon.ico" rel="shortcut icon" type="image/vnd.microsoft.icon" />
<link rel="apple-touch-icon-precomposed" href="/gl/templates/yoo_nano2/apple_touch_icon.png" />
```

## 360浏览器的兼容模式
在Head中添加一行代码可以修改客户端360浏览器的兼容模式：
```
<meta name=”renderer”content=”webkit|ie-comp|ie-stand”>
```
content的取值为webkit，ie-comp，ie-stand之一，区分大小写，分别代表用极速模式，兼容模式，IE模式打开。

## 媒体查询动态过渡效果
```
-webkit-transition: all 1s ease;  
-moz-transition: all 1s ease; 
-o-transition: all 1s ease; 
transition: all 1s ease; 
```