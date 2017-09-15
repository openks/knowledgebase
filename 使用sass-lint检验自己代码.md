---
title: 使用sass-lint 检验自己代码  
date: 2017-06-02 16:57:16  
description: 使用sass-lint 检验自己代码并处理error及警告信息    
keywords: [前端,scss,sass,lint,sass-lint]  
---
在给ATOM装上`linter-sass-lint`之后，校验了下虽然木有错但是出现了很多的警告信息。  
分类下具体包含以下几种  
```
Warning	sass-lint	quotes Strings must use single quotes	1:9
Warning	sass-lint	indentation Mixed tabs and spaces	6:2
Warning	sass-lint	class-name-format Class '.content_wrap' should be written in lowercase with hyphens	10:3
Warning	sass-lint	force-element-nesting Type-selector should be nested within its parent Pseudo-class	18:4
Warning	sass-lint	force-element-nesting Class should be nested within its parent Class	24:4
Warning	sass-lint	force-pseudo-nesting Pseudo-class should be nested within its parent Type-selector	18:4
Warning	sass-lint	force-pseudo-nesting Pseudo-class should be nested within its parent Class	64:4
Warning	sass-lint	property-sort-order Expected `margin-bottom`, found `padding`	34:3
Warning	sass-lint	no-color-literals Color literals such as '#e6e6e6' should only be used in variable declarations	43:21
Warning	sass-lint	extends-before-declarations Extends should come before declarations	71:4
Warning	sass-lint	border-zero A value of `none` is not allowed. `0` must be used.	135:13
```
针对每一种类型查看警告说明结果如下   
[文件引入默认使用单引号](https://github.com/sasstools/sass-lint/blob/master/docs/rules/quotes.md)  
[缩进不能混合tab和空格（默认两个空格）](https://github.com/sasstools/sass-lint/blob/master/docs/rules/indentation.md)  
[类名格式默认用连字符链接的纯小写字母 ](https://github.com/sasstools/sass-lint/blob/master/docs/rules/class-name-format.md)  
[强制元素嵌套 ](https://github.com/sasstools/sass-lint/blob/master/docs/rules/force-element-nesting.md)  
[强制伪类嵌套 ](https://github.com/sasstools/sass-lint/blob/master/docs/rules/force-pseudo-nesting.md)  
[属性排序（默认用字母排序） ](https://github.com/sasstools/sass-lint/blob/master/docs/rules/property-sort-order.md)  
[无颜色字面量（默认统一使用变量） ](https://github.com/sasstools/sass-lint/tree/master/docs/rules/no-color-literals.md)  
[继承放到第一个 ](https://github.com/sasstools/sass-lint/tree/master/docs/rules/extends-before-declarations.md)  
[默认无边框设置`border:0;` ](https://github.com/sasstools/sass-lint/blob/master/docs/rules/border-zero.md)  
