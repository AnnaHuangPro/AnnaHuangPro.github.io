---
layout:       post
title:        "CSS的absolute绝对布局相对于哪个父级元素定位的分析"
subtitle:     "border对定位的影响、margin-top"
date:         2018-08-19 12:00:00
author:       "AnnaHuang"
header-img:   ""
header-mask:  0.3
catalog:      true
multilingual: false  
tags:
    - css
    - 前端开发
    
---

使用absolute布局的同学有时候可能会疑惑，有时设置为absolute定位的元素相对父元素（最近的父级元素）定位，有时候不是。那么，绝对布局的元素到底相对哪个父级元素定位呢？

> 绝对定位的元素，相对于static定位以外的第一个父元素进行定位。
> 元素默认的定位值是static，所以往上找参照元素一直到根元素 。

下面我们用实践一步步分析。
### 1.父级元素position为默认值，显然，蓝色的子元素没有相对红色的父级元素定位

```
<div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
    <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
</div>
```
![此处输入图片的描述][1]

用Chrome的开发调试功能分析，发现蓝色的子元素相对页面顶部定位了：
![此处输入图片的描述][2]

### 2.再加一层父级元素，蓝色子元素依然是相对页面顶部定位：
```
<div>
    <div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
        <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
    </div>
</div>
```

![此处输入图片的描述][3]
不是相对<body>节点定位：
![此处输入图片的描述][4]
而是相对<html>节点的顶部：
![此处输入图片的描述][5]

### 3.加3层父节点，父节点的position都依然是默认值，蓝色子元素依然是相对页面顶部定位：
```
<div>
    <div>
        <div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
            <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
        </div>
    </div>
</div>
```

![此处输入图片的描述][6]

### 4. 将第2个父节点的position设置为static，蓝色子元素依然是相对页面顶部定位：
```
<div>
    <div style="position: static">
        <div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
            <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
        </div>
    </div>
</div>
```

![此处输入图片的描述][7]
### 5. 将所有div父节点的position设置为static，蓝色子元素依然是相对页面顶部定位：
```
<div style="position: static">
    <div style="position: static">
        <div style="position: static; width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
            <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
        </div>
    </div>
</div>
```

![此处输入图片的描述][8]

### 6. 将蓝色子元素的直接父节点的position设置为relative，这时蓝色子元素相对直接父节点定位了：
```
<div style="position: relative; width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
    <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
</div>
```

![此处输入图片的描述][9]
### 7. 将蓝色子元素的直接父节点position为默认，间接父节点设置为relative，这时：
```
<div style="position: relative">
    <div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
        <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
    </div>
</div>
```
![此处输入图片的描述][10]
蓝色div在红色div的左边，好像既不相对红色元素定位，也不相对页面顶部定位：
![此处输入图片的描述][11]
但我们发现设置position为relative的父节点没有设置大小，我们不妨设置一下它的背景色+透明度，好分析是不是相对relative的父节点定位的：
```
<div style="position: relative; background: yellowgreen; opacity: 0.5">
    <div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
        <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
    </div>
</div>
```
如图，半透明+黄绿色的div就是设置position为relative的父节点：
![此处输入图片的描述][12]
我们发现这个position为relative的节点并没有设置width和height，显然，默认的width为100%，即直接父节点的width；而默认的height为auto，自动装满最大height的子元素，即100px。

于是我们再设置一下这个relative position父节点的width和height：
```
<div style="position: relative; width: 150px; height: 150px;">
    <div style="position: static; width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
        <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
    </div>
</div>
```
设置width和height为150px后，relative position的父级元素的margin-top自动和子元素的margin-top对齐
![此处输入图片的描述][13]
我们设置一下背景色和透明图，以便更好地分析：
```
<div style="position: relative; width: 150px; height: 150px; background: yellowgreen; opacity: 0.5">
    <div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
        <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
    </div>
</div>
```
黄绿色+半透明的div即为relativeposition的父节点，虽然没有设置margin-top，但自动和红色div的margin-top对齐：
![此处输入图片的描述][14]

### 8. 为relative position的父节点设置border：

前面实战例子中的relative position父节点都没有设置border，我们探索一下设置border对定位的影响。

步骤7我们已经分析过，没有设置margin-top的div父节点会自动对齐子div的margin-top：
![此处输入图片的描述][15]
我们只在relative position的父节点设置了border: 1px solid black：
```
<div style="position: relative; border: 1px solid black">
    <div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
        <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
    </div>
</div>
```
设置了border之后，没有设置relativeposition父节点的margin-top，margin-top默认为0，宽度100%；高度自动包含所有子元素：
![此处输入图片的描述][16]
设置宽度和高度：
```
<div style="position: relative; width: 150px; height: 150px; border: 1px solid black">
    <div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
        <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
    </div>
</div>
```
设置了border和宽度之后，margin-top默认为0：
![此处输入图片的描述][17]
我们再试着设置margin-top：
```
<div style="position: relative; width: 150px; height: 150px; margin-top: 50px; margin-left: 50px; border: 1px solid black">
    <div style="width: 100px; height: 100px; margin-top: 100px; margin-left: 100px; background: red;">
        <div style="position: absolute; top: 20px; left: 20px; width: 50px; height: 50px; background: blue"></div>
    </div>
</div>
```
设置了margin-top和margin-left之后，relative position父div位置随之发生改变，而蓝色子div也相对重新定位：
![此处输入图片的描述][18]

### 9. 设置position为absolute，但不指定left、top、right、bottom的值:
这时该div还是在父节点的流内，布局效果相当于指定父节点position: relative，并且div本身的left: 0， top: 0。



# 总结

通过以上8步实战分析，我们总结出以下3点：
> 1.绝对定位的元素，相对于static定位以外的第一个父元素进行定位。

> 2.没有设置border和margin-top的父div，顶部自动对齐margin-top最大的子div的顶部

> 3.设置了border但没有设置margin-top的父div，margin-top默认为0


[1]: https://upload-images.jianshu.io/upload_images/2226455-00009e37e7e95e97.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[2]: https://upload-images.jianshu.io/upload_images/2226455-f63a4463a2eede52.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[3]: https://upload-images.jianshu.io/upload_images/2226455-4bf3829ab206cf71.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[4]: https://upload-images.jianshu.io/upload_images/2226455-e343ecbc7d4cdfa2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[5]: https://upload-images.jianshu.io/upload_images/2226455-2aff402953aff99a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[6]: https://upload-images.jianshu.io/upload_images/2226455-7d79c3a5cc767776.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[7]: https://upload-images.jianshu.io/upload_images/2226455-aaecbf967f94b9b0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[8]: https://upload-images.jianshu.io/upload_images/2226455-bf888706e9f089ab.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[9]: https://upload-images.jianshu.io/upload_images/2226455-6d19122d94e8de3a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[10]: https://upload-images.jianshu.io/upload_images/2226455-fe3e270de4657c31.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[11]: https://upload-images.jianshu.io/upload_images/2226455-e2baf6ed768a26a7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[12]: https://upload-images.jianshu.io/upload_images/2226455-18fe99b855e1c5f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[13]: https://upload-images.jianshu.io/upload_images/2226455-a5044e0af6a84db8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[14]: https://upload-images.jianshu.io/upload_images/2226455-7603f8321db56bf0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[15]: https://upload-images.jianshu.io/upload_images/2226455-e2baf6ed768a26a7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[16]: https://upload-images.jianshu.io/upload_images/2226455-a0610232c642a43a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[17]: https://upload-images.jianshu.io/upload_images/2226455-a937ac1b8f447c7c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700
[18]: https://upload-images.jianshu.io/upload_images/2226455-3c5eeef95d42e889.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700