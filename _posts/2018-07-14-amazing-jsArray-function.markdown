---
layout:       post
title:        "感觉很神奇的js数组操作方法"
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

> 在使用中发现神奇，随时更新





## Array.prototype.splice()

### 代码示例

```javascript
var months = ['Jan', 'March', 'April', 'June'];
months.splice(1, 0, 'Feb');
// inserts at 1st index position(插入到下标为1的数组元素里，删除0个元素)
console.log(months);
// expected output: Array ['Jan', 'Feb', 'March', 'April', 'June']

months.splice(4, 1, 'May');
// replaces 1 element at 4th index(将下标为4的元素替换成‘May’)
console.log(months);
// expected output: Array ['Jan', 'Feb', 'March', 'April', 'May']
```



### 定义和用法

splice()方法用于**插入**、**删除**或**替换**数组的元素

**注意：这种方法会改变原始数组！**

splice(index,len,[item])    注释：该方法会改变原始数组。

splice有3个参数，它也可以用来替换/删除/添加数组内某一个或者几个值

> index:数组开始下标       

> len: 替换/删除的长度       

> item:替换的值，(删除操作的话 item为空)

如：arr = ['a','b','c','d']

#### 删除 ----  item不设置

arr.splice(1,1)   //['a','c','d']         删除起始下标为1，长度为1的一个值，len设置的1，如果为0，则数组不变

arr.splice(1,2)  //['a','d']          删除起始下标为1，长度为2的一个值，len设置的2

#### 替换 ---- item为替换的值

arr.splice(1,1,'ttt')        //['a','ttt','c','d']         替换起始下标为1，长度为1的一个值为‘ttt’，len设置的1

arr.splice(1,2,'ttt')        //['a','ttt','d']         替换起始下标为1，长度为2的两个值为‘ttt’，len设置的1

#### 添加 ----  len设置为0，item为添加的值

arr.splice(1,0,'ttt')        //['a','ttt','b','c','d']         表示在下标为1处添加一项‘ttt’



### 返回值

Array。

 若是删除了元素，则返回含有删除了的元素的数组



## Array.prototype.slice()

### 代码示例

```javascript
var animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]

console.log(animals.slice(1, 5));
// expected output: Array ["bison", "camel", "duck", "elephant"]

```

### 定义和用法

slice() 方法可从已有的数组中返回选定的元素。

slice() 方法可提取字符串的某个部分，并以 **新的字符串** 返回被提取的部分。

*array*.slice(*start*, *end*)  **注意：end元素不包括**

| 参数      | 描述                                       |
| ------- | ---------------------------------------- |
| *start* | 可选。规定从何处开始选取。如果是负数，那么它规定从数组尾部开始算起的位置。也就是说，-1 指最后一个元素，-2 指倒数第二个元素，以此类推。 |
| *end*   | 可选。规定从何处结束选取。该参数是数组片断结束处的数组下标。如果没有指定该参数，那么切分的数组包含从 start 到数组结束的所有元素。如果这个参数是负数，那么它规定的是从数组尾部开始算起的元素。 |

#### 参数为负数的例子

```javascript
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var myBest = fruits.slice(-3,-1);

//out:
//Lemon,Apple
```

**注意：** slice() 方法不会改变原始数组。



## Array.prototype.concat()

#### 代码示例

```javascript
var hege = ["Cecilie", "Lone"];
var stale = ["Emil", "Tobias", "Linus"];
var kai = ["Robin"];
var children = hege.concat(stale,kai);

//out:
//Cecilie,Lone,Emil,Tobias,Linus,Robin
```



## Array.prototype.concat()

#### 定义和用法

concat() 方法用于连接两个或多个数组。

该方法不会改变现有的数组，而仅仅会返回被连接数组的一个 **副本** 。

**注意：参数可以是数组也可以是具体的值，可以是任意多个** 



## Array.prototype.reduce()



## Array.prototype.join()

### 代码示例

```javascript
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var energy = fruits.join();
var energy = fruits.join(" ");
var energy = fruits.join(" and ");

//out:
//Banana,Orange,Apple,Mango
//Banana Orange Apple Mango
//Banana and Orange and Apple and Mango
```

### 定义和用法

join() 方法用于把数组中的所有元素转换一个字符串

元素是通过指定的分隔符进行分隔的。



## Array.prototype.push()

### 定义和用法

push()方法可向数组的末尾添加一个或多个元素，并返回**新长度**

**提示：在数组的起始位置添加元素请使用unshift()方法**



## Array.prototype.pop()

### 定义和用法

pop()方法用于删除数组的最后一个元素并返回删除的元素

**提示：要删除数组的第一个元素请使用shift()方法**



## Array.prototype.unshift()

### 定义和用法

unshift() 方法可向数组的开头添加一个或更多元素，并返回新的长度。

**提示：在数组的末尾添加元素请使用push()方法** 



## Array.prototype.shift()

### 定义和用法

shift() 方法用于把数组的第一个元素从其中删除，并返回第一个元素的值。如果数组是空，那么shift()方法将不进行任何操作，返回undefined值。

**注意：该方法不创建新数组，而是直接修改原有的arrayObject.**

**提示：在数组的末尾删除元素请使用pop()方法**

 

# 数组的遍历

## Array.prototype.filter()

### 代码示例

```javascript
var words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]
```

### 定义和用法

filter()方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素



**注意：filter()不会对空数组进行检测；filter()不会改变原始数组**



> filter()方法的参数是函数，并且数组中的每个元素都会执行这个函数
>
> 这个函数有一个参数是必须的->currentValue,表明当前的值。



## Array.prototype.map()

### 代码示例

```javascript
var numbers = [4, 9, 16, 25];

function myFunction() {
    x = document.getElementById("demo")
    x.innerHTML = numbers.map(Math.sqrt);
}

//out:
//2,3,4,5
```

### 定义和用法

map()方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值

map()方法会按照原始数组元素顺序依次处理元素

**注意：map()不会对空数组进行检测** 

**注意：map()不会改变原始数组**

 

## Array.prototype.forEach()

### 代码示例

```javascript
var array1 = ['a', 'b', 'c'];

array1.forEach(function(element) {
  console.log(element);
});

// expected output: "a"
// expected output: "b"
// expected output: "c"
```



### 另一段代码示例

```javascript
function Counter() {
    this.sum = 0;
    this.count = 0;
}

Counter.prototype.add = function(array) {
    array.forEach(function(entry) {
        this.sum += entry;
        ++this.count;
    }, this);
    //console.log(this);
};

var obj = new Counter();
obj.add([1, 3, 5, 7]);

obj.count; 
// 4 === (1+1+1+1)
obj.sum;
// 16 === (1+3+5+7)
```

*thisValue* 参数：可选。传递给函数的值一般用 "this" 值。
如果这个参数为空， "undefined" 会传递给 "this" 值



>  注意：没有办法中止或者跳出 forEach 循环，除了抛出一个异常。如果你需要这样，使用forEach()方法是错误的，你可以用一个简单的循环作为替代。如果您正在测试一个数组里的元素是否符合某条件，且需要返回一个布尔值，那么可使用 [`Array.every`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/every) 或 [`Array.some`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/some)。如果可用，新方法 [`find()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/find) 或者[`findIndex()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex) 也可被用于真值测试的提早终止。



forEach()函数可以执行函数内的语句，可见可以改变原数组



### map与forEach的区别

1、forEach()返回值是undefined，不可以链式调用。

2、map()返回一个新数组，原数组不会改变。

3、没有办法终止或者跳出forEach()循环，除非抛出异常，所以想执行一个数组是否满足什么条件，返回布尔值，可以用一般的for循环实现，或者用Array.every()或者Array.some();

4、$.each()方法规定为每个匹配元素规定运行的函数，可以返回 false 可用于及早停止循环。



## Array.prototype.every()

### 定义和用法

`**every()**` 方法测试数组的所有元素是否都通过了指定函数的测试。

demo

```javascript
function isBelowThreshold(currentValue) {
  return currentValue < 40;
}

var array1 = [1, 30, 39, 29, 10, 13];

console.log(array1.every(isBelowThreshold));
// expected output: true
```

### 描述

`every` 方法为数组中的每个元素执行一次 `callback` 函数，直到它找到一个使 `callback` 返回 *false*（表示可转换为布尔值 false 的值）的元素。如果发现了一个这样的元素，`every` 方法将会立即返回 `false`。否则，`callback` 为每一个元素返回 `true`，`every` 就会返回 `true`。`callback` 只会为那些已经被赋值的索引调用。不会为那些被删除或从来没被赋值的索引调用。

**注意：`every` 和数学中的"所有"类似，当所有的元素都符合条件才返回true。**

​	**另外，空数组也是返回true。(空数组中所有元素都符合给定的条件，注：因为空数组没有元素)。** 



## Array.prototype.some()

### 定义和用法

`**some()**` 方法测试数组中的 **某些** 元素是否通过由提供的函数实现的测试。

```javascript
var array = [1, 2, 3, 4, 5];

var even = function(element) {
  // checks whether an element is even
  return element % 2 === 0;
};

console.log(array.some(even));
// expected output: true
```

### 描述

`some` 为数组中的每一个元素执行一次 `callback` 函数，直到找到一个使得 callback 返回一个“真值”（即可转换为布尔值 true 的值）。如果找到了这样一个值，`some` 将会**立即**返回 `true`。否则，`some` 返回 `false`。`callback`只会在那些”有值“的索引上被调用，不会在那些被删除或从来未被赋值的索引上调用。

`some` 被调用时不会改变数组。



## Array.prototype.find()(ES6)

## Array.prototype.find()(ES6)





## Array.findIndex()(ES6)