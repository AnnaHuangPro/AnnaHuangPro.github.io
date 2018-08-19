---
layout:       post
title:        "使用vue如何默认选中单选框"
subtitle:     ""
date:         2018-08-09 12:00:00
author:       "AnnaHuang"
header-img:   ""
header-mask:  0.3
catalog:      false
multilingual: false  
tags:
    - JavaScript框架vue
    - 前端开发
    
---

> 使用了vue以后，发现这真的是一个灵活高效的框架，能够轻松实现页面的实时刷新。

那么，今天先聊聊单选框的使用。一般我们使用单选框，会这么写：
```
//HTML
<input type="radio" name="radios" value="1" checked><label>one</label>
<br>
<input type="radio" name="radios" value="2"><label>two</label>
<br>
<input type="radio" name="radios" value="2"><label>three</label>
```

有”checked”属性的单选框会默认选中。

但在vue里这是无效的，因为它会跟具体的参数值绑定。（后来看到vue的官网教程，确实写了这么一段：**v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值**。因为它会选择 Vue 实例数据来作为具体的值。你应该通过 JavaScript 在组件的 data 选项中声明初始值。）

```
//HTML
<input type="radio" name="radios" value="1" v-model="param"><label>one</label>
<br>
<input type="radio" name="radios" value="2" v-model="param"><label>two</label>
<br>
<input type="radio" name="radios" value="3" v-model="param"><label>three</label>
```

```
//JS
export default{
    data(){
        return{
            param:'1' //设置默认值为1，即设置第一个单选框为选中状态
        }
    }
}
```
