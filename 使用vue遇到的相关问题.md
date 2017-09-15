---
title: 使用vue遇到的相关问题
date: 2017-05-25 14:44:15  
description: 记录使用vue遇到的相关问题
---

## 1.Vue页面加载时`Mustache`标签尚未编译时导致页面闪烁问题
解决方法1：  
使用`v-cloak`  
```css
[v-cloak] {
  display: none;
}
```
```html
<div v-cloak>
  {{ message }}
</div>
```
[详见这里](https://cn.vuejs.org/v2/api/#v-cloak)  
解决方法2：  
使用`v-text`
```html
<span v-text="msg"></span>
<!-- same as -->
<span>{{msg}}</span>
```
[详见这里](https://cn.vuejs.org/v2/api/#v-cloak)  

## 2.判断Vue页面运行环境
如果使用vue.js则表示使用开发环境如果使用vue.min.js则表示生产环境  
在开发环境下devtools默认开启，在生产环境下则devtools默认关闭  
在页面就可以使用`Vue.config.devtools`的值进行判断。
```
Vue.config.devtools = true
```
[详见这里](https://cn.vuejs.org/v2/api/#devtools)
