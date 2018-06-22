---
layout:       post
title:        "本地存储之WebStorage"
subtitle:     ""
date:         2018-06-22 12:00:00
author:       "AnnaHuang"
header-img:   ""
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - 前端开发
    - JavaScript
    - 缓存
    - 编码中学习
---

> HTML5中与本地存储相关的两个重要内容：Web Storage与本地数据库。其中，Web Storage存储机制是对HTML4中cookie存储机制的一个改善。由于cookie存储机制有很多缺点，HTML5不再使用它，转而使用改良后的Web Storage存储机制。本地数据库是HTML5中新增的一个功能，使用它可以在客户端本地建立一个数据库，原本必须保存在服务器端数据库中的内容现在可以直接保存在客户端本地了，这大大减轻了服务器端的负担，同时也加快了访问数据的速度。

本文主要来讲解Web Storage

我们知道，在HTML4中可以使用cookie在客户端保存诸如用户名等简单的用户信息，但是，通过长期的使用，你会发现，用cookie存储永久数据存在以下几个问题：

1.大小：cookie的大小被限制在4KB。

2.带宽：cookie是随HTTP事务一起被发送的，因此会浪费一部分发送cookie时使用的带宽。

3.复杂性：要正确的操纵cookie是很困难的。

针对这些问题，在HTML5中，重新提供了一种在客户端本地保存数据的功能，它就是 **Web Storage**。

具体来说，Web Storage又分为两种：

1.**sessionStorage**：将数据保存在session对象中。所谓session，是指用户在浏览某个网站时，**从进入网站到浏览器关闭所经过的这段时间**，也就是用户浏览这个网站所花费的时间。session对象可以用来保存在这段时间内所要求保存的任何数据。

2.**localStorage**：将数据保存在客户端本地的硬件设备(通常指硬盘，也可以是其他硬件设备)中，即使浏览器被关闭了，该数据仍然存在，下次打开浏览器访问网站时仍然可以继续使用。

这两者的区别在于，sessionStorage为**临时**保存，而localStorage为**永久**保存。

到目前为止，Firefox3.6以上、Chrome6以上、Safari 5以上、Pera10.50以上、IE8以上版本的浏览器支持sessionStorage与localStorage的使用。

接下来具体看一下sessionStorage与localStorage的使用示例。

首先，准备一个用来保存数据和显示数据的网页

```
<!DOCTYPE html>  
<html>  
    <head>  
    <meta charset="UTF-8">  
    <title>Web Storage 示例</title>  
    </head>  
    <body>  
    <h1>Web Storage 示例</h1>  
    <p id="msg"></p>  
    <input type="text" id="input" />  
    <input type="button" value="保存数据" onclick="saveStorage('input');" />  
    <input type="button" value="读取数据" onclick="loadStorage('msg');" />  
    </body>  
</html>  
```
单击"保存数据"按钮时调用saveStorage方法保存数据，单击"读取数据"按钮时调用loadStorage方法调用数据，这两个方法均在脚本文件script.js中，如下：
```
//sessionStorage 示例  (保存一个会话周期:从打开浏览器——到关闭浏览器窗口)  
function saveStorage(id){  
    var target=document.getElementById(id);  
    var str=target.value;  
    sessionStorage.setItem("message",str);  
    //或者sessionStorage.message=str;  
}  
function loadStorage(id){  
    var target=document.getElementById(id);  
    var msg=sessionStorage.getItem("message");  
    //或者var msg=sessionStorage.message;  
    target.innerHTML=msg;  
}  
//localStorage 示例(可永久保存)      
function saveStorage(id){  
    var target=document.getElementById(id);  
    var str=target.value;  
    localStorage.setItem("message",str);  
    //或者localStorage.message=str;  
}  
function loadStorage(id){  
    var target=document.getElementById("msg");  
    var msg=localStorage.getItem("message");  
    //或者var msg=localStorage.message;  
    target.innerHTML=msg;  
}  
```

这个脚本文件分别使用了sessionStorage与localStorage两种方法。这两种方法都是当用户在input文本框中输入内容后单击"保存数据"按钮保存数据，单击"读取数据"按钮读取保存后的数据。但是两种方法对数据的处理方式不一样:
    **在使用sessionStorage方法时，如果关闭了浏览器，这个数据就丢失了，下一次打开浏览器单击"读取数据"按钮时，读取不到任何数据**。
    在使用localStorage方法时，即使浏览器关闭了，下次打开浏览器时仍然能够读取保存的数据。不过，**数据保存是按不同的浏览器分别进行保存的**,也就是说，打开别的浏览器是读取不到在这个浏览器中保存的数据的。
    


下面具体看一下读写数据时使用的基本方法
(1)sessionStorage
保存数据的方法：
```
sessionStorage.setItem("key","value");  
//或者写成  
sessionStorage.key="value";  
```
读取数据的方法：
```
变量=sessionStorage.getItem("key");  
//或者写成  
变量=sessionStorage.key;  
```

(2)localStorage
保存数据的方法：
```
localStorage.setItem("key","value");  
//或者写成  
localStorage.key="value";  

```
读取数据的方法：
```
变量=localStorage.getItem("key");  
//或者写成  
变量=localStorage.key;  

```

## 总结

在保存数据时，若使用sessionStorage读取或保存数据，则使用sessionStorage对象并调用该对象的读写方法；
若使用localStorage读取或保存数据，则使用localStorage对象并调用该对象的读写方法。
在进行读写时，不管是哪个对象，都可以通过该对象的getItem方法来读取数据，也可以该对象的自定义属性值读取数据；可以通过该对象的setItem方法保存数据，也可以通过该对象的自定义属性值保存数据。保存数据时按“键名/键值”的形式进行保存。当通过该对象的getItem方法读取数据时，将参数指定为键名，该方法返回键值；当通过该对象的自定义属性值读取数据时，可以将该对象的某个自定义属性名作为键名，访问该自定义属性的属性值即可得到键值；当通过该对象的setItem方法保存数据时，将第一个参数指定为键名，将第二个参数指定为键值；当通过该对象的自定义属性值保存数据时，可以将该对象的某个自定义属性名作为键名，然后直接将该自定义属性值设置为键值。

**注意：在保存数据时不允许重复保存相同的键名。保存后可以修改键值，但不允许修改键名(只能重新取键名，然后再保存键值)。**
