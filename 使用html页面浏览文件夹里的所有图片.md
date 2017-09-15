---
title: 使用html页面浏览文件夹里的所有图片
date: 2016-9-18 17:27:35
tags: [前端]
categories: [前端]
description: 通过nodejs使用html页面浏览文件夹里的所有图片
keywords: [前端，nodejs,html]
---


使用html页面浏览文件夹里的所有图片

# 实现思路

使用node服务器读取某文件夹下面的所有图片文件的文件名(jpg,png,jpeg,gif等,暂时只想到了这几个)，遍历所有文件名拼接字符串，使用生成的字符串填充新生成的html文件即可。

第一步：获取文件夹下的所有文件名
```js
var fs = require("fs");
var path = require("path");
var m = fs.readdirSync(__dirname);
console.log(m);//会打印文件夹下所有文件名及文件夹名称，m为数组
```
第二步：判断是不是图片,并拼接`html`字符串
```js
m[i].indexOf(".jpg") > 0
```
第三步：把生成的字符串`str`填充到`index.html`文件中
```js
fs.writeFile("index.html", str, function(e) {
  console.log("文件创建完成");
})
```
第四步：把文件夹名字作为`html`的`title`
查看path的api找到有一个[parse](https://nodejs.org/api/path.html#path_path_parse_path)方法：
```js
path.parse(__dirname).name
//测试结果如下(dirname的name属性即父文件夹的名字)
//F:\code\personal\openks>node test.js
dirname:
 { root: 'F:\\',
  dir: 'F:\\code\\personal',
  base: 'openks',
  ext: '',
  name: 'openks' }
filename:
 { root: 'F:\\',
  dir: 'F:\\code\\personal\\openks',
  base: 'test.js',
  ext: '.js',
  name: 'test' }
```

完整代码及使用方法详见[这里](http://openks.oschina.io/js/create-index.js)
