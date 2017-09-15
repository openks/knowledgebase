---
title: 开发利器之atom  
date: 2016-10-17 16:20:55  
tags: [工具]  
categories: [前端]  
description: GitHub 发布的文本编辑器 Atom 介绍  
keywords: [前端，工具，atom]  
---

打开设置
windows
 file / settings
mac
 atom / Preferences

安装插件列表  
minimap   缩略图  
docblockr   jsdoc注释快速生成器  
pigments   颜色显示插件  
atom-jsfmt   格式化js插件   
atom-css-comb css,less,scss,sass文件格式化  
atom-beautify   格式化html,css,js,java,SQL 等  
open_in_cmd  右键可以直接在cmd命令行中打开  
sass-autocomplie 自动编译sass文件  
script     run code in atom  
node-debugger debugger for nodejs  
atom-ternjs js自动补全引擎Adds support for ES5, ES6, ES7, Node.js, jQuery, Angular and more. Extendable via plugin  
emmet html快速生成器  
git-status 显示git状态  
git-time-machine git时光机  



代码折叠
```
折叠一行 ctrl+alt+[
展开折叠  ctrl+alt+]
折叠多行选中后 ctrl+alt+shift+[
展开多行折叠  ctrl+alt+shift+]
调出F12控制台 ctrl+alt+i
一次保存多个文件 ctrl+shift+s
```

```
keybinding
'atom-text-editor:not([mini])':
  #删除行
  'ctrl-d': 'editor:delete-line'
'atom-workspace':
  #格式化
  'ctrl-shift-f': 'editor:auto-indent'
  #向下复制行
  'ctrl-alt-down': 'editor:duplicate-lines'
```

## Q&A
Q1:保存文件自动格式化  
A1:插件`atom-beautify`设置里有选项可以进行设置  

Q2:ATOM一行显示多个字符，不自动换行  
A2:
```
//勾选后自动换行,取消勾选就不换行
Soft Wrap
//勾选后可自主设置每行显示字符数
Soft Wrap At Preferred Line Length
//自助设置的每行最多显示字数
Preferred Line Length
```


Q3:ATOM如何一次保存所有修改过的文件  
A3:这个没找到，一个文件一个文件的保存吧,一个文件也就一次`ctrl+s`而已

Q4:apm里显示npm版本过低如何升级  
A4:重新下载AOM并安装即可
