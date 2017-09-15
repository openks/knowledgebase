---
title: atom在线插件安装失败解决办法  
date: 2017-3-23 13:16:06    
description: atom在线插件安装失败解决办法即atom离线安装插件方法   
---


atom通过setting中无法下载插件，通过apm也无法下载插件，可能是网络、config配置的问题，不好解决。

下面的方法全手动，基本属于万金油方法：

1，在atom的setting页面中点击open config folder进入到atom的配置项目，然后我们转到该项目下的windows窗口进入package文件夹，这将是我们放置插件包的地方。

2，打开https://atom.io/packages浏览自己需要的插件

3，选择插件中的repo,复制github地址

4，在步骤一的文件夹中运行`git bash`命令

输入以下：
```
git clone https://github.com/emmetio/emmet-atom （将插件的源代码克隆到文件夹中）
//也可以直接在github页面下载release版本的源代码 一般为zip文件 //解压到atom的package文件夹
cd emmet-atom （进入插件包文件夹 即package文件夹子目录）
npm install （下载，生成插件 推荐使用cnpm）
```
5，重启atom 或者 `control + alt + R` 刷新atom
