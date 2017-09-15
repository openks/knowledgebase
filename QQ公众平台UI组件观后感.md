---
title: QQ公众平台UI组件相关文章观后感
date: 2016-3-18 15:29:32
tags: [前端]
categories: [前端]
description: hexo博客搭建相关信息及插件配置等
keywords: []
---


## QQ公众平台UI组件相关文章观后感

### 无意间的发现
最近在做项目时总是苦于类名不知道怎么合适的取，就总在想web组件化后应该可以缓解或者说解决这种问题
无意间发现了这篇文章 [顺势而为，HTML发展与UI组件设计进化](http://web.jobbole.com/84929/)读后感触颇深，基于HTML标准开发ui的确这个蛮好的，
优点明显更强的语义化，可访问性，便于SEO,学习成本低，可以一次性全局处理

### 有了发现就有成长 
1.文本域 
这个就没啥了，验证和校验结果融合在一起，如果想使用自带验证就要想办法修改校验不通过的默认样式
添加padding值使文本域显得够大，字体在中间大小合适
2.单选框 
第一次知道一个radio可以有两个(多于一个)label指向它，一个label做前面的选择状态，一个label做文本内容展示
3.数字框 
加减箭头去掉(demo里没找到相关代码，自己百度了个)，但是向上向下的键盘功能还是有的
```css
//chrome											
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none !important;
  margin: 0;
}
//firefox
input[type="number"]{
	-moz-appearance:textfield;
}
```
4.下拉框重做
```html
div>(a>span+i)+(ul>li)
```
a标签占据整个下拉框span里是选中的内容，i为下拉箭头，ul为下拉后的内容
5.复选框
与单选框类似
6.switch开关
简单讲就是一个复选框（只有一个选项的复选框组）
通过使用一个label和伪元素模拟两个层一大一小，根据选中状态设置小层在大层上的位置


label的选中状态在css里就是用`:checked`过滤选中的元素，用相邻兄弟选择器加号(需要复选框在要定位的`label`标签的上一个统计元素，位置不能变)来定位`label`
`:checked+.labelClassName`  因css选择器是从右向左执行所以效率还是蛮高的

### 启示
1.两个简单对象完全可以用伪元素来解决
2.选中未选中label样式完全可以在css里使用相邻兄弟选择器来解决

