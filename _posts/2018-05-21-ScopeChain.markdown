---
layout:       post
title:        "对作用域的认识"
subtitle:     ""
date:         2018-05-21 12:00:00
author:       "AnnaHuang"
header-img:   ""
header-mask:  0.3
catalog:      true
multilingual: false 
tags:
    - 前端开发
    - JavaScript

---
## 作用域链
在红宝书中对作用域链的描述一段话中有这么一段话：**当代码在一个环境中执行时，会创建变量对象的一个作用域链。作用域链的用途是保证对执行环境有权
访问的所有变量和函数的有序访问。** 作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其**活动对象**作为变量对象。
活动对象在最开始时只包含一个变量，即arguments对象（这个对象在全局环境中是不存在的）。作用域链中的下一个变量对象来自包含（外部）环境，而再下一个变量
对象则来自下一个包含环境。这样，一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域中的最后一个对象。

> 这里有很多概念，比如什么是执行环境、变量对象等。先解释这些概念

### 执行环境(Execution Context)
执行环境（execution context,为简单起见，有时也称为“环境”）是Javascript中最为重要的一个概念。执行环境定义了变量或函数有权访问的其他数据，决定了
它们各自的行为。每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。虽然我们编写的代码无法访问这个对象，但解析器在
处理数据时会在后台使用它。
<br>
全局执行环境是最外围的一个执行环境。根据ECMAScript实现所在的宿主环境不同，表示执行环境的对象也不一样。在Web浏览器中，全局执行环境被认为是window对象，
因此所有的全局变量和函数都是作为window对象的属性和方法创建的。某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁（
全局执行环境直到应用程序退出——例如关闭网页或浏览器——时才会被销毁）。
<br>
每个函数都有自己的执行环境。当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。
ECMAScript程序中的执行流正是由这个方便的机制控制着。
### 变量对象（Variable Object）
每一个执行环境都对应一个变量对象，在该执行环境中定义的所有变量和函数都存放在其对应的变量对象中。
1. 进入执行上下文中，VO的初始化过程如下：
<br>
函数的形参：变量对象的一个属性，其属性名就是形参的名字，其值就是实参的值；对于没有传递的参数，其值为undefined；
<br>
函数声明:变量对象的一个属性，其属性名和属性值都是函数对象创建出来的，如果变量对象已经有了相同名字的属性，则替换它的值；
<br>
变量声明：变量对象的一个属性，其属性名即为变量名，其值为undefined；如果变量名和已经声明的函数名或者函数的参数名相同，则不会影响已经存在的属性。
2. 执行代码阶段，变量对象中的一些属性undefined值将会确定

> 函数表达式不包含在变量对象中

``` 
    var foo = 10;
    
    function bar(){} // function declaration, FD  
    (function baz() {}); // function expression, FE  
    
    console.log(
        this.foo == foo, // true  
        widow.bar == bar // true
    );
    
    console.log(baz);  // ReferenceError, "baz" is not defined
```

之后，全局上下文的变量对象为
![VO](/img/in-post/VO.png)

### 活动对象
当函数被调用的时候，一个特殊的对象–活动对象将会被创建。这个对象中包含形参和arguments对象。活动对象之后会作为函数上下文的变量对象来使用。换句话说，活动对象除了变量和函数声明之外，它还存储了形参和arguments对象。

## 作用域详解
由以上介绍可知，当某个函数被调用时，会创建一个执行环境及相应的作用域链。然后，使用arguments和其他命名参数的值来初始化函数的活动对象。但在作用域链中，外部函数的活动对象始终处于第二位，外部函数的外部函数对象处于第三位……直至作为作用域终点的全局执行环境
```
    function compare(value1,value2){
        if(value1 < value2){
            return -1;
        } else if( value1 > value2 ) {
            return 1;
        } else {
            return 0;
        }
    }
    
    var result = compare(1,2);
    
```

以上代码定义了compare()函数，然后又在全局作用域中调用了它。当调用compare()时，会创建一个包含arguments、value1、value2的活动对象。
全局执行环境的变量对象（包含result和compare）在compare()执行环境的作用域链中则处于第二位。
下图包含了上述关系的compare()函数执行时的作用域链。
![作用域链](/img/in-post/scope-chain.jpg)

后台的每个执行环境都有一个表示变量的对象——变量对象。全局环境的变量对象始终存在，而像compare()函数这样的局部环境的变量对象，则**只在函数执行的过程中存在**。
在创建compare()函数时，会创建一个预先包含全局变量对象的作用域链，这个作用域链会被保存在内部的[[Scope]]属性中。**当调用compare()函数时，会为函数创建一个执行环境，
然后通过赋值函数的[[Scope]]属性中的对象构建起执行环境的作用域链**。此后，又有一个活动对象被创建并被推入执行环境作用域链的前端。对于这个例子中，compare()函数的执行函数而言，
其作用域链中包含两个变量对象：本地活动对象和全局变量对象。**作用域链本质上是一个指向变量对象的指针列表，它只引用但不实际包含变量对象。**

### 闭包与作用域链
无论什么时候在函数中访问一个变量时，就会从作用域链中搜索具有相应名字的变量。一般来讲，当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域（全局执行环境的变量对象）。但是闭包的情况又有所不同。
```
    function createComparisionFunction(propertyName) {
        return function(object1,object2) {
            var value1 = object1[propertyName];
            var value2 = object2[propertyName];
            if(value1 < value2){
                return -1;
            } else if( value1 > value2 ) {
                return 1;
            } else {
                return 0;
            }
        }
    }
    
    //创建函数（返回的是函数）
    var compare = createComparisionFunction("name");
    
    //调用函数
    var result = compare({name:"AnnaHuang"},{name:"JamesZhu"});
    
    //解除对匿名函数的引用，以便释放内存
    compare = null;
```
当上述代码执行时，下图展示了包含函数与内部匿名函数的作用域链

![closureAndScopeChain](/img/in-post/closureAndScopeChain.jpg)
在匿名函数从createComparisonFunction()中被返回后，**它的作用域链被初始化为包含createComparisonFunction()函数的活动对象和全局变量对象**。
这样，匿名函数就可以访问在createComparisonFunction()中定义的所有变量。更为重要的是， createComparisonFunction()函数在执行完毕后，其活动对象也不会被销毁，
因为匿名函数的作用域链仍然在引用这个活动对象。即当createComparisonFunction()函数返回后，**其执行环境的作用域链会被销毁，但它的活动对象任然会留在内存中**；
直到匿名函数被销毁后，createComparisonFunction()的活动对象才会被销毁。

### 作用域链知识总结

当代码在一个环境中执行时，都会创建一个作用域链。 作用域链的用途是保证对执行环境有权访问的所有变量和函数的有序访问。整个作用域链的本质是一个指向变量对象的指针列表。作用域链的最前端，始终是当前正在执行的代码所在环境的变量对象。
<br>
如果这个环境是函数，则将其活动对象（activation object)作为变量对象。活动对象在最开始时只包含一个变量，就是函数内部的arguments对象。作用域链中的下一个变量对象来自该函数的包含环境，而再下一个变量对象来自再下一个包含环境。这样，一直延续到全局执行环境，全局执行环境的变量对象始终是作用域链中的最后一个对象。



