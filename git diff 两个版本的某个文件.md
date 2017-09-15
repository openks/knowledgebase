---
title:  git diff 两个版本的某个文件  
date: 2017-06-02 14:27:39  
description:  git比较两个版本的某一个文件详细变化内容  
keywords: [git,git命令]    
---

昨天遇到一个奇怪的问题，其实也不复杂，就是对一个`input`添加日历控件，所有js代码也都写好了只需要加上个`class`就成了结果这个日历控件死活不出来。结果同事从能正常显示控件的地儿复制了一段竟然好了。问他改了哪里他说不知道，他说不知道没改动啥东西我就想到了文件版本对比。  

方法一：还是我最常用的命令行

```
git diff <commitA> <commitB> filename
```

方法二：在gitlab上对比查看  

输入两个提交版本，会显示所有文件的版本差异相同的则不显示,具体如下图
![image](http://7xl9u9.com1.z0.glb.clouddn.com/WX20170602-140742@2x.png)

再上一张我没找到问题的文件对比图，这确实是什么都没改啊（加了class这个除外）
![image](http://7xl9u9.com1.z0.glb.clouddn.com/WX20170602-141302@2x.png)
