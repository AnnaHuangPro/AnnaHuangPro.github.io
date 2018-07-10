---
layout:       post
title:        "vue.js开发环境搭建"
subtitle:     ""
date:         2018-07-10 12:00:00
author:       "AnnaHuang"
header-img:   ""
header-mask:  0.3
catalog:      true
multilingual: false 
tags:
    - 前端开发
    - vue
---
# 步骤
## 安装vue的脚手架

npm install -g vue-cli

> vue-cli 是vue.js的脚手架，用于自动生成vue.js+webpack的项目模板，分为vue init webpack-simple 项目名 和vue init webpack 项目名 两种。

> 当然首先你得安装vue，webpack，node等一些必要的环境

## 使用vue-cli初始化项目(webpack的方案)

> vue init webpack my-project

这个命令的意思是初始化一个项目，其中Webpack是构建工具，也就是整个项目是基于webpack的。

目前，这个项目还只是一个结构框架，整个项目需要的依赖资源都还没有安装。

## 进入到目录

> cd my-project

## 安装依赖

> npm install

package.json就是你项目所有的依赖声明，依赖会自动安装在node_modules文件夹中。

## 开始运行

> npm run dev

其中"run"对应的是package.json文件中，scripts字段中的dev，也就是node build/dev-server.js命令的一个快捷方式。
项目运行成功后，浏览器会自动打开localhost:8080（如果浏览器没有自动打开，可手动打开）
