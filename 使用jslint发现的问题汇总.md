---
title: 使用jslint发现的问题汇总
date: 2016-4-1 16:46:46
tags: [前端]
categories: [前端]
description: 写js代码常见问题
keywords: [前端，jslint,js,常见错误]
---

主要包括以下几个方面问题
1.eval
2.===与==
3.Missing semicolon
4.Bad line breaking
5.Unnecessary semicolon
6.Script URL
7.Expected an assignment or function call and instead saw an expression.


```js

static\js\data-schedule.js: line 31, col 28, eval can be harmful.

if (data instanceof Object) {
	jsondata = data;
} else {
	jsondata = eval('(' + data + ')');
}

//eval函数已经不建议使用了 这里完全可以用JSON.parse()来取代
static\js\data-schedule.js: line 35, col 26, Use '===' to compare with 'null'
static\js\data-schedule.js: line 127, col 26, Use '!==' to compare with 'undefined'.
static\js\forget.js: line 7, col 23, Use '===' to compare with 'false'.

if(check_flag == false)

static\js\forget.js: line 145, col 23, Use '===' to compare with '0'.

if (waitemail == 0)

//类型确定的比较最好使用=== 我还是常用== 习惯应该改正了

static\js\data-schedule.js: line 129, col 56, Script URL.

$('.lt').attr('href', 'javascript:queryMatchs('+up+',1)');

//href里写javascript: 这种写法我从来没想到过，我想的是给a标签添加click事件

iframe.src == "javascript:'%3Chtml%3E%3C/html%3E';"
//src里写javascript:

static\js\data-schedule.js: line 164, col 6, Missing semicolon.
//缺少分号 这个我一般都不会少的
static\js\data-schedule.js: line 168, col 13, Bad line breaking before '-'.

str.substr(4
- RegExp.$1.length));

//话说这种断行我是第一次见 两行变一行就行了

static\js\data-schedule.js: line 174, col 17, Bad line breaking before '?'.
//?:三目运算符分成三行我也是第一次
static\js\data-shooter.js: line 13, col 2, Unnecessary semicolon.
function hello(){
	console.log("hello");
};//这里如果有分号就会提示 Unnecessary semicolon.

static\js\footer.js: line 1, col 1, document.write can be a form of eval.

document.writeln('<div class="footer">')
document.writeln('CopyRight©2015 上超联赛有限公司 All Rigths Reserved 沪ICP备15015551号-1')
document.writeln('</div>')

//这种写法可以用DocumentFragment 然后找到body 调用appendChild()方法即可


static\js\forget.js: line 168, col 14, Expected an assignment or function call and instead saw an expression.

var wait=90;
timeB = false,
scountdown = false; // line 168

//声明之间用逗号隔开一不小心就会写成分号了
```
