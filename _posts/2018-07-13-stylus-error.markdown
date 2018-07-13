---
layout:       post
title:        "在vue中首次使用stylus的安装方法"
subtitle:     ""
date:         2018-07-12 12:00:00
author:       "AnnaHuang"
header-img:   ""
header-mask:  0.3
catalog:      false
multilingual: false  
tags:
    - vue.js
    - css预处理器
---

项目npm run dev时报错：
![项目npm run dev时报错][1]
解决方案

    npm install stylus-loader css-loader style-loader --save-dev
    
安装以后还会报错

继续执行

    npm install stylus
    
然后执行

    npm run dev
    
成功解决

> 因此在首次初始化的时候进行如下步骤的安装
1.npm install stylus --save-dev
2.npm install stylus-loader --save-dev
3.npm run dev（每次改动了配置文件均需要重新启动项目）


  [1]: https://annahuangpro.github.io/images/20187/13/stylus-error.png