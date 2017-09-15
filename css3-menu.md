---
title: 用 CSS3 和 JavaScript 制作径向动画菜单
date: 2016-6-24 14:41:23
tags: [前端]
categories: [前端]
description: 用 CSS3 和 JavaScript 制作径向动画菜单
keywords: [前端，cssc,ss3,js,JavaScript,动画菜单]
---
# 用 CSS3 和 JavaScript 制作径向动画菜单

先看demo

<p data-height="308" data-theme-id="0" data-slug-hash="WxRQJP" data-default-tab="result" data-user="openks" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/openks/pen/WxRQJP/">圆形动画菜单</a> by openks (<a href="http://codepen.io/openks">@openks</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

html和css都不难，很容易实现，主要问题在于子图标个数和子图标之间的角度不一样
所有子节点都在O点，使用绝对定位后设置子节点的top和left值即可
以A点为例子(其中圆点为50%,50%)
```js
left=(50+AD距离)*100%
top=(50-AE距离)*100%
```
一旦理解了这些就简单多了附图一张（其中n为子节点个数,i为第i个节点）
![图片](http://7xl9u9.com1.z0.glb.clouddn.com/2016-06-24_15-18-30.png);
来源在[这里](http://codepen.io/CreativePunch/pen/lAHiu)
只不过我一直没看懂它到底是如何计算left和top的，不过思路应该差不多
