---
layout:       post
title:        "事件循环"
subtitle:     ""
date:         2018-06-20 12:00:00
author:       "AnnaHuang"
header-img:   ""
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - 前端开发
    - css
    - 编码中学习
---



## 什么是事件循环

Event Loop 是一个很重要的概念，指的是计算机系统的一种运行机制。

JavaScript语言就采用这种机制，来解决单线程运行带来的一些问题。

想要理解Event Loop，就要从程序的运行模式讲起。运行以后的程序叫做["进程"](http://zh.wikipedia.org/wiki/%E8%BF%9B%E7%A8%8B)（process），一般情况下，一个进程一次只能执行一个任务。

如果有很多任务需要执行，这个时候就有三种解决方案：

+ 排队。因为一个进程一次只能执行一个任务，只好等前面的任务执行完了，在执行后面的任务。
+ 新建进程。使用fork命令，为每个任务新建一个进程。
+ 新建线程。因为进程太耗费资源了，所以如今的程序往往允许一个进程包含多个线程，由线程去完成任务。



​	以javaScript语言为例，它是一种单线程语言，所有任务都在一个线程上完成，即采用上面的第一种方法。一旦遇到大量任务或者遇到一个耗时的任务，网页就会出现假死，因为javaScript停不下来，也就无法响应用户的行为。

​	你也许会问，javaScript为什么是单线程，难道就不能实现多线程吗？

​	这跟历史有关。JavaScript从诞生起就是单线程。原因大概是不想让浏览器变得太复杂，因为多线程需要共享资源，且有可能修改彼此的运行结果，对于一种网页脚本语言来说，这就太复杂了。后来就约定俗成，JavaScript为一种单线程语言。（Worker API可以实现多线程，但是JavaScript始终都是多线程的。）

​	如果某个任务很耗时，比如涉及很多的I/O（输入输出）操作，那么线程的运行大概是下面这样。

​	上图的绿色部分是程序的运行时间，红色部分是等待时间。可以看到，由于I/O操作很慢，所以这个线程的大部分时间运行时间都在空等I/O操作返回的结果，这种运行方式成为“同步模式”（Synchronous	I/O）或“堵塞模式”（blocking I/O）

​	如果采用多线程，同时运行多个任务，那很可能就是下面这样。

​	上图表明，多线程不仅占用多倍的系统资源，也闲置多倍的资源，这显然是不合理的。

​	EventLoop就是为了解决这个问题而提出的。[Wikipedia](http://en.wikipedia.org/wiki/Event_loop)这样定义：

> "**Event Loop是一个程序结构，用于等待和发送消息和事件。**（a programming construct that waits for and dispatches events or messages in a program.）"



简单说，就是在程序中设置两个线程：一个负责程序本身的运行，成为“主线程”；另一个负责主线程与其他进程（主要是各种I/O操作）的通信，被称为“Event Loop线程”（可以译为“消息进程”）。

