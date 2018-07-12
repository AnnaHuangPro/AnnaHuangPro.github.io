---
layout:       post
title:        "相对路径和绝对路径的最终版解析"
subtitle:     ""
date:         2018-07-12 12:00:00
author:       "AnnaHuang"
header-img:   ""
header-mask:  0.3
catalog:      true
multilingual: false  
tags:
    - 整理的小知识
---
## 绝对路径

绝对路径不解释，就是完整描述文件位置的路径就是绝对路径。

如：

    E:\wo\造轮子\vue\TODO\src\assets\img\logo.png

> 在文件路径中复制而来的，是反斜杠。这是因为windows系统已经使用斜杠/作为DOS命令的提示符的参数标志了，为了不混淆所以使用反斜杠\作为路径分隔符。随着法阵，DOS系统已经被淘汰了，命令提示符也用的很少，斜杠和反斜杠在大多数情况下可以互换。

    https://www.pckings.net/img/photo.jpg

#### 扩充小知识点，斜杠'/'和反斜杠'\'

 - 浏览器地址栏中的网址使用 斜杠/ 作为路径分隔符 
 - Windows文件浏览器使用 反斜杠\作为路径分隔符 出现在html
 - url()属性中的路径，指定的是网络路径，所以必须用斜杠/
 如：
```
//如果url后面用反斜杠\，则不会显示任何背景
<div style="background-image:url(/Image/Control/title.png);background-repeat:repeat-x;padding:10px 10px"></div>
```
 - 出现在普通字符串中的路径，如果表示Windows文件路径，则使用斜杠/和反斜杠\是一样的；如果代表的是网络文件路径，则必须使用斜杠/
 如：
```
<!--本地路径/和\是等效的-->
<img src=".\Image\20161025\guo.jpg" />
<img src="./Image/20161025/guo.jpg" />
<img src=".\Image/20161025/guo.jpg" />
<img src="./Image\20161025\guo.jpg" />
<!--网络文件路径一定要使用反斜杠\-->
<img src="http://img6.bdstatic.com/img/image/smallpic/chongwu10120.jpg"
```

## 相对路径
**小总结：**

 - ./Images/这样写表示，当前目录中的Images文件夹
 > ./表示当前路径，在通常情况下可以省略，只有在特殊情况下不能省略，即"./Images/"可以写成"Images/"

 - ../Images/这样写表示，当前目录的上一层目录中的Images文件夹
 
 > 我们使用"../"来表示上一级目录，"../../"表示上上级的目录，以此类推。

 - /Images/这样写表示，项目根目录（可以指磁盘跟目录，也可以指项目根目录，据实际情况而定）
 
