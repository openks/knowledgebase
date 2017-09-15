---
title:  ES6新的数据类型Symbol   
date: 2017-03-17 13:21:35  
description:  ES6新的数据类型Symbol了解     
keywords: [前端,ES6,Symbol]  
---


ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）

Symbol 值通过Symbol函数生成。生成的值是独一无二的

Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分

```js
var s1 = Symbol('foo');
var s2 = Symbol('bar');
var s3 = Symbol('bar');
s1 // Symbol(foo)
s2 // Symbol(bar)
s2==s3 //false
s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```
如果 Symbol 的参数是一个对象，就会调用该对象的toString方法，将其转为字符串，然后才生成一个 Symbol 值。

Symbol 值不能与其他类型的值进行运算,但是，Symbol 值可以显式转为字符串
```js
var sym = Symbol('My symbol');

"your symbol is " + sym
// TypeError: can't convert symbol to string
`your symbol is ${sym}`
// TypeError: can't convert symbol to string

var sym = Symbol();
Boolean(sym) // true
!sym  // false

if (sym) {
  // ...
}

Number(sym) // TypeError
sym + 2 // TypeError
```
Symbol 值作为对象属性名时，不能用点运算符，该属性还是公开属性，不是私有属性,该属性不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。但是，它也不是私有属性，有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有 Symbol 属性名。

Object.getOwnPropertySymbols方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。

Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

我们希望重新使用同一个Symbol值，Symbol.for方法可以做到这一点。它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的Symbol值。如果有，就返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值
```js
var s1 = Symbol.for('foo');
var s2 = Symbol.for('foo');

s1 === s2 // true
Symbol.for("bar") === Symbol.for("bar")
// true

Symbol("bar") === Symbol("bar")
// false
```

Symbol.for()与Symbol()这两种写法，都会生成新的Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。Symbol.for()不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的key是否已经存在，如果不存在才会新建一个值。比如，如果你调用Symbol.for("cat")30次，每次都会返回同一个 Symbol 值，但是调用Symbol("cat")30次，会返回30个不同的Symbol值。
Symbol.keyFor方法返回一个已登记的 Symbol 类型值的key。

```js
var s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

var s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

Symbol.for为Symbol值登记的名字，是全局环境的，可以在不同的 iframe 或 service worker 中取到同一个值  
除了定义自己使用的Symbol值以外，ES6还提供了11个内置的Symbol值，指向语言内部使用的方法。  
