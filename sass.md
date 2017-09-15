---
title: sass学习
date: 2016-5-26 11:36:22
tags: [前端]
categories: [前端]
description: sass 基础知识学习及整理
keywords: [朱阳星,前端,openks,sass]
---

## sass的文件后缀
sass文件后缀有两种即sass和scss
sass:不使用大括号和分号
scss:使用大括号和分号
```sass
//sass语法
body
  background: #eee
  font-size:12px
  p
    background: #0982c1
```
```scss
//scss语法
body {
  background: #eee;
  font-size:12px;
  p{
    background: #0982c1;
  }
}
```
## 文件导入
1.导入sass文件可以省略文件后缀
2.导入sass文件会在编译是合并为一个文件
3.`_base.scss`导入时可以不写下划线即可以`@import base`
4.导入css文件以@import方式存在,跟普通CSS导入样式文件一样


```scss
//a.scss
body {
  background: #eee;
}
```
```scss
//b.scss
@import "reset.css";
@import "a";
p{
  background: #0982c1;
}
```
```css
/*编译后*/
@import "reset.css";
body {
  background: #eee;
}
p{
  background: #0982c1;
}
```
## 注释
`/**/`和`//`
单行注释不会输入到编译产生的CSS中
```scss
/**
 * 设置统一样式
 */
body{
  color: red;//color red
}
```

```css
/**
 * 设置统一样式
 */
body {
  color: red;
}
```

## 变量

### 普通变量
普通变量定义之后可以在全局范围内使用。
```scss
//sass style
$fontSize: 12px;
body{
    font-size:$fontSize;
}

//css style
body{
    font-size:12px;
}
```

### 默认变量
sass的默认变量仅需要在值后面加上!default即可。
```scss
//sass style
$baseLineHeight:        2; //写在前面覆盖默认值
$baseLineHeight:        1.5 !default;
body{
    line-height: $baseLineHeight;
}

//css style
body{
    line-height:2;
}
```
### 特殊变量
如果变量作为属性或在某些特殊情况下等则必须要以#{$variables}形式使用。
```scss
//sass style
$borderDirection:       top !default;
$baseFontSize:          12px !default;
$baseLineHeight:        1.5 !default;

//应用于class和属性
.border-#{$borderDirection}{
  border-#{$borderDirection}:1px solid #ccc;
}
//应用于复杂的属性值
body{
    font:#{$baseFontSize}/#{$baseLineHeight};
}

//css style
.border-top{
  border-top:1px solid #ccc;
}
body {
  font: 12px/1.5;
}
```

### 多值变量
多值变量分为list类型和map类型,list类型有点像js中的数组，而map类型有点像js中的对象
```scss
//sass style
//一维数据
//$px: 5px 10px 20px 30px;
//二维数据，相当于js中的二维数组
//$px: 5px 10px, 20px 30px;
//用nth($var,$index)取值
$linkColor:  #08c #333 !default;//第一个值为默认值，第二个鼠标滑过值
a{
  color:nth($linkColor,1);
  &:hover{
    color:nth($linkColor,2);
  }
}

//css style
a{
  color:#08c;
}
a:hover{
  color:#333;
}
```
```scss
//sass style
//$map: (key1: value1, key2: value2, key3: value3);
$headings: (h1: 2em, h2: 1.5em, h3: 1.2em);
@each $header, $size in $headings {
  #{$header} {
    font-size: $size;
  }
}

//css style
h1 {
  font-size: 2em;
}
h2 {
  font-size: 1.5em;
}
h3 {
  font-size: 1.2em;
}
```

## 嵌套
选择器的嵌套和属性的嵌套
### 选择器嵌套
```scss
//sass style
//-------------------------------
#top_nav{
  background-color:#333;
  li{
    float:left;
  }
  a{
    color: #fff;
    &:hover{
      color:#ddd;
    }
  }
}

//css style
#top_nav{
  background-color:#333;
}  
#top_nav li{
  float:left;
}
#top_nav a{
  color: #fff;
}
#top_nav a:hover{
  color:#ddd;
}
```

### 属性嵌套
```scss
//sass style
.fakeshadow {
  border: {
    style: solid;
    left: {
      color: #888;
    }
    right: {
      color: #ccc;
    }
  }
}

//css style
.fakeshadow {
  border-style: solid;
  border-left-color: #888;
  border-right-color: #ccc;
}
```

## 混合(mixin)

无参数
```scss
//sass style
@mixin center-block {
    margin:0 auto;
}
.demo{
    @include center-block;
}

//css style
.demo{
    margin:0 auto;
}
```
有参数mixin
```scss
//sass style

@mixin opacity($opacity:50) {
  opacity: $opacity / 100;
  filter: alpha(opacity=$opacity);
}
.opacity{
  @include opacity; //参数使用默认值
}
.opacity-80{
  @include opacity(80); //传递参数
}
//css style
.opacity {
  opacity: 0.5;
  filter: alpha(opacity=50);
}
.opacity-80 {
  opacity: 0.8;
  filter: alpha(opacity=80);
}
```

多个参数mixin

调用时可直接传入值，如@include传入参数的个数小于@mixin定义参数的个数，则按照顺序表示，后面不足的使用默认值，如不足的没有默认值则报错。除此之外还可以选择性的传入参数，使用参数名与值同时传入。

```scss
//sass style
@mixin horizontal-line($border:1px dashed #ccc, $padding:10px){
    border-bottom:$border;
    padding-top:$padding;
}
.imgtext-h li{
    @include horizontal-line(1px solid #ccc);
}
.imgtext-h--product li{
    @include horizontal-line($padding:15px);
}

//css style
.imgtext-h li {
    border-bottom: 1px solid #cccccc;
    padding-top: 10px;
}
.imgtext-h--product li {
    border-bottom: 1px dashed #cccccc;
    padding-top: 15px;
}
```

多组值参数mixin
在变量后加三个点表示，如$variables...。
```scss
//sass style
//box-shadow可以有多组值，所以在变量参数后面添加...
@mixin box-shadow($shadow...) {
  -webkit-box-shadow:$shadow;
  box-shadow:$shadow;
}
.box{
  border:1px solid #ccc;
  @include box-shadow(0 2px 2px rgba(0,0,0,.3),0 3px 3px rgba(0,0,0,.3),0 4px 4px rgba(0,0,0,.3));
}

//css style
.box{
  border:1px solid #ccc;
  -webkit-box-shadow:0 2px 2px rgba(0,0,0,.3),0 3px 3px rgba(0,0,0,.3),0 4px 4px rgba(0,0,0,.3);
  box-shadow:0 2px 2px rgba(0,0,0,.3),0 3px 3px rgba(0,0,0,.3),0 4px 4px rgba(0,0,0,.3);
}
```

## 继承

### 继承@extend
```scss
//不管是否使用了@extend去继承相应的样式，都会解析出来所有的样式
//@mixin通过@include调用后解析出来的样式是以拷贝形式存在的，而@extend则是以联合声明的方式存在的，
//所以从3.2.0版本以后，建议传递参数的用@mixin，而非传递参数类的使用占位选择器%。

//sass style
h1{
  border: 4px solid #ff9aa9;
}
.speaker{
  @extend h1;
  border-width: 2px;
}

//css style
h1,.speaker{
  border: 4px solid #ff9aa9;
}
.speaker{
  border-width: 2px;
}
```

### 占位选择器%
```scss
//占位选择器%如果不调用则不会有任何多余的css文件
//sass style
%ir{
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}
%border5{
  border: 5px;
}
#header{
  h1{
    @extend %ir;
    width:300px;
  }
}
.ir{
  @extend %ir;
}

//css style
#header h1,.ir{
  color: transparent;
  text-shadow: none;
  background-color: transparent;
  border: 0;
}
#header h1{
  width:300px;
}
```

## 函数

```scss
//sass style
$baseFontSize:      10px !default;

// pixels to rems
@function pxToRem($px) {
  @return $px / $baseFontSize * 1rem;
}

body{
  font-size:$baseFontSize;
}
.test{
  font-size:pxToRem(16px);
}

//css style
body{
  font-size:10px;
}
.test{
  font-size:1.6rem;
}
```

## 运算

```scss
//可以对数值型的Value(如：数字、颜色、变量等)进行加减乘除四则运算。运算符前后需留一个空格
$baseFontSize:          14px !default;
$baseLineHeight:        1.5 !default;
$baseGap:               $baseFontSize * $baseLineHeight !default;
$halfBaseGap:           $baseGap / 2  !default;
$samllFontSize:         $baseFontSize - 2px  !default;
```
## 条件判断及循环

###  if判断@if判断
```scss
//sass style
$lte7: true;
$type: monster;
.ib{
    display:inline-block;
    @if $lte7 {
        *display:inline;
        *zoom:1;
    }
}
p {
  @if $type == ocean {
    color: blue;
  } @else if $type == monster {
    color: green;
  }
}

//css style
.ib{
    display:inline-block;
    *display:inline;
    *zoom:1;
}
p {
  color: green;
}
```

### 三目判断
```scss
//if($condition, $if_true, $if_false)
if(true, 1px, 2px) => 1px
if(false, 1px, 2px) => 2px
```

###  for循环
```scss
// @for $i from <start> through <end> $i表示变量  through包括end这个数
// @for $i from <start> to <end> $i表示变量  to不包括end这个数

//sass style
@for $i from 1 through 3 {
  .item-#{$i} { width: 2em * $i; }
}

//css style
.item-1 {
  width: 2em;
}
.item-2 {
  width: 4em;
}
.item-3 {
  width: 6em;
}
```

### 循环,@each循环
```scss
//@each $var in <list or map>。其中$var表示变量，而list和map表示list类型数据和map类型数据
//单字段list循环
//sass style
$animal-list: puma, sea-slug, egret, salamander;
@each $animal in $animal-list {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}

//css style
.puma-icon {
  background-image: url('/images/puma.png');
}
.sea-slug-icon {
  background-image: url('/images/sea-slug.png');
}
.egret-icon {
  background-image: url('/images/egret.png');
}
.salamander-icon {
  background-image: url('/images/salamander.png');
}
```
```scss
//多字段list循环
//sass style
$animal-data: (puma, black, default),(sea-slug, blue, pointer),(egret, white, move);
@each $animal, $color, $cursor in $animal-data {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
    border: 2px solid $color;
    cursor: $cursor;
  }
}

//css style
.puma-icon {
  background-image: url('/images/puma.png');
  border: 2px solid black;
  cursor: default;
}
.sea-slug-icon {
  background-image: url('/images/sea-slug.png');
  border: 2px solid blue;
  cursor: pointer;
}
.egret-icon {
  background-image: url('/images/egret.png');
  border: 2px solid white;
  cursor: move;
}
```

```scss
//多个字段map数据循环
//sass style
$headings: (h1: 2em, h2: 1.5em, h3: 1.2em);
@each $header, $size in $headings {
  #{$header} {
    font-size: $size;
  }
}

//css style
h1 {
  font-size: 2em;
}
h2 {
  font-size: 1.5em;
}
h3 {
  font-size: 1.2em;
}
```
>[参考链接](http://www.w3cplus.com/sassguide/syntax.html)
