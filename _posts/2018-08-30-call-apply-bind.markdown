---
layout:       post
title:        "bind、call、apply之讲解"
subtitle:     ""
date:         2018-07-13 12:00:00
author:       "AnnaHuang"
header-img:   ""
header-mask:  0.3
catalog:      true
multilingual: false  
tags:
    - JavaScript
    - 前端开发
---

## call与apply的区别与相同之处

### 作用

改变对象的执行上下文（改变this指向）

### 为什么需要改变执行上下文？

例子1：我有一张银行卡，只有我知道密码，所以只有我取钱，此时银行卡的“执行上下文”是我；而之后我把密码告诉了老婆大人，那么老婆大人知道密码以后也就可以从这张卡取钱，老婆大人取钱的时候，“执行上下文”就变成了老婆大人。

例子2：小明有一个炒菜的铲子，小明的室友小刚今天突然想自己做菜吃，但是小刚没有铲子。小刚又不想为了做个菜单独买把铲子，于是就借用了小明的铲子，这样既达到了目的，又节省了开支，一举两得。

### 基本使用

```javascript
function.call(obj[,arg1[, arg2[, [,.argN]]]]])
```

### call()

+ 调用`call`的对象必须是个函数function
+ `call`的第一个参数将会是function改变上下文后指向的对象，也就是上面例子里的小刚，也就是上上面例子里的老婆大人，如果不传，将会默认是全局对象`window`
+ 第二个参数开始可以接收任意个参数，这些参数将会作为function的参数传入function
+ 调用`call`的方法会立即执行



### apply()

```javascript
function.apply(obj[,argArray])
```

与`call`方法的使用基本一致，但是只接收两个参数，其中第二个参数必须是一个**数组**或者**类数组**，这也是这两个方法很重要的一个区别

#### **数组与类数组小科普**

数组我们都知道是什么，它的特征都有哪些呢？

1. 可以通过角标调用，如 `array[0]`
2. 具有长度属性`length`
3. 可以通过 for 循环和`forEach`方法进行遍历

类数组顾名思义，具备的特征应该与数组基本相同，那么可以知道，一个形如下面这个对象的对象就是一个类数组

```javascript
var arrayLike = {
    0: 'item1',
    1: 'item2',
    2: 'item3',
    length: 3
}
```

类数组`arrayLike`可以通过角标进行调用，具有`length`属性，同时也可以通过 for 循环进行遍历

我们经常使用的获取dom节点的方法返回的就是一个类数组，在一个方法中使用 `arguments`关键字获取到的该方法的所有参数也是一个类数组

但是类数组却不能通过`forEach`进行遍历，因为`forEach`是数组原型链上的方法，类数组毕竟不是数组，所以无法使用.

#### 类数组转化为数组

```javascript
var arr = Array.prototype.slice.call(arguments);
var arr = [].slice.call(arguments);
```

其他几种方法

#### Array.from()

是ES6中的方法，用于将类数组转换为数组。

```
var arr = Array.from(arguments);1
```

只要有length属性的对象，都可以应用此方法转换成数组。

#### 扩展运算符

ES6中的扩展运算符`...`也能将某些数据结构转换成数组，这种数据结构必须有**遍历器** 接口。

```
var args = [...arguments];
```

#### $.makeArray()

jQuery的此方法可以将类数组对象转化为真正的数组

```
var arr = $.makeArray(arguments);
```



### 代码示例

```javascript
function fn(a,b,c){
  console.log(a,b,c,this);
}
fn.call(null,1,2,3);//区别在于call需要将参数一一罗列出来
fn.apply(null,[1,2,3]);//apply用数组，即使一个参数也要用数组
//如果第一个参数为null/undefined,则this指向window(在node环境中则指向global)
```



### 区别就在于传参时参数的类型

### 相同点

改变对象的执行上下文（改变this指向）

### 异同点

`call`方法从第二个参数开始可以接收任意个参数，每个参数会映射到相应位置的func的参数上，可以通过参数名调用，但是如果将所有的参数作为数组传入，它们会作为一个整体映射到func对应的第一个参数上，之后参数都为空.

`apply`方法最多只有两个参数，第二个参数接收数组或者类数组，但是都会被转换成类数组传入func中，并且会被映射到func对应的参数上



## 【其他用途——对象继承】

由于可以改变`this`的指向，所以也就可以实现对象的继承

```
function superClass () {
    this.a = 1;
    this.print = function () {
        console.log(this.a);
    }
}

function subClass () {
    superClass.call(this);
    this.print();
}

subClass();
// 1
```

`subClass`通过`call`方法，继承了`superClass`的`print`方法和`a`变量，同时`subClass`还可以扩展自己的其他方法