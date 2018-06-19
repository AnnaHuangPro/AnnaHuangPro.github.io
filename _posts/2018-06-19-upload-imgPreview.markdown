---
layout:       post
title:        "html5图片上传与预览实现"
subtitle:     ""
date:         2018-06-18 12:00:00
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

最近做项目需要用到图片上传与预览功能，由于是用于手机端，所以研究了下H5的实现方式。

## 图片预览
首先，解决图片预览问题。在html5中，提供了FileReader来读取本地文件，使我们可以实现图片预览功能。


## FileReader
+ 属性，所有属性都是只读的：
   + FileReader.error，读取文件时，出现的DOMError。
   + FileReader.readyState，读取状态；0，没有数据加载；1，数据正在加载；2，读取已经完成。
   + FileReader.result，文件内容；该属性只在读取操作完成后才有效，并且格式取决于读取时使用的方法。
+ 事件
   + FileReader.onabort，读取操作中止。
   + FileReader.onerror，读取出现错误。
   + FileReader.onload，读取成功完成后。
   + FileReader.onloadstart，读取开始。
   + FileReader.onloadend，读取完成，无论是否读取成功。
   + FileReader.onprogress，当读取Blob内容时。
+ 方法
   + FileReader.abort() 
     中止读取。然后readyState变为2。
   + FileReader.readAsArrayBuffer() 
     将文件读取成ArrayBuffer。
   + FileReader.readAsBinaryString() 
     读取成二进制字符串。
   + FileReader.readAsDataURL() 
     读取成DataURL。
   + FileReader.readAsText() 
     读取成文本。

## 预览功能实现
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>图片上传预览</title>
<script type="text/javascript">
    function imgPreview(fileDom){
        //判断是否支持FileReader
        if (window.FileReader) {
            var reader = new FileReader();
        } else {
            alert("您的设备不支持图片预览功能，如需该功能请升级您的设备！");
        }

        //获取文件
        var file = fileDom.files[0];
        var imageType = /^image\//;
        //是否是图片
        if (!imageType.test(file.type)) {
            alert("请选择图片！");
            return;
        }
        //读取完成
        reader.onload = function(e) {
            //获取图片dom
            var img = document.getElementById("preview");
            //图片路径设置为读取的图片
            img.src = e.target.result;
        };
        reader.readAsDataURL(file);
    }
</script>
</head>
<body>
    <img id="preview" />
    <br />
    <input type="file" name="file" onchange="imgPreview(this)" />
</body>
</html>
```
效果预览：
![](/img/in-post/upload-imagePreview.png)

## 上传功能
```
    function upload() {
        var xhr = new XMLHttpRequest();
        var progress = document.getElementById("progress")
        progress.style.display = "block";

        xhr.upload.addEventListener("progress", function(e) {
            if (e.lengthComputable) {
                var percentage = Math.round((e.loaded * 100) / e.total);
                progress.value = percentage;
            }
        }, false);

        xhr.upload.addEventListener("load", function(e){
              console.log("上传完毕...")
          }, false);

        xhr.open("POST", "upload");
        xhr.overrideMimeType('text/plain; charset=x-user-defined-binary');
        xhr.onreadystatechange = function() {
            if (xhr.readyState == 4 && xhr.status == 200) {
                alert(xhr.responseText); // handle response.
                progress.style.display = "none";
                progress.value = 0;
            }
        };
        var file = document.getElementById("imgFile");
        var fd = new FormData();
        fd.append(file.files[0].name, file.files[0]);
        xhr.send(fd);
    }
```