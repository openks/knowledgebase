---
title: 读airbnb标准有感  
date: 2017年06月07日15:06:57  
description: 读airbnb标准有感  
keywords: [js标准,airbnb标准]  
---
读[airbnb javascript标准](https://github.com/airbnb/javascript) 有感

在读标准时发现还有很多知识点不大明白记录之
```
Array.map
Array.forEach
Array.reduce
Array.from
Object.assign
Number(string)
object literals
export 和export default区别
import foo, { named1, named2 } from 'foo';
```

## Array.map介绍
```
var new_array = arr.map(callback[, thisArg])

callback
        Function that produces an element of the new Array, taking three arguments:
    currentValue 当前值
        The current element being processed in the array.
    index index值 从0开始的index值
        The index of the current element being processed in the array.
    array 当前数组
        The array map was called upon.
thisArg
    Optional. Value to use as this when executing callback.
    If a thisArg parameter is provided to map, it will be used as callback's this value. Otherwise, the value undefined will be used as its this value.
Return value  返回值为新数组不影响原数组
    A new array with each element being the result of the callback function.

Array.map不修改原数组
```

## Array.forEach介绍
```
arr.forEach(function callback(currentValue, index, array) {
    //your iterator
}[, thisArg]);

callback
        Function to execute for each element, taking three arguments:
    currentValue  当前值
        The current element being processed in the array.
    index  从0开始的index值
        The index of the current element being processed in the array.
    array 当前数组
        The array that forEach() is being applied to.
thisArg [Optional]
        Value to use as this (i.e the reference Object) when executing callback.
        If a thisArg parameter is provided to forEach, it will be used as callback's this value. Otherwise, the value undefined will be used as its this value.
Return value
    undefined.
Array.forEach不修改原数组
```
## Array.reduce介绍
```
arr.reduce(callback, [initialValue])

callback
        Function to execute on each element in the array, taking four arguments:
    accumulator 累加器
        The accumulator accumulates the callback's return values; it is the accumulated value previously returned in the last invocation of the callback, or initialValue, if supplied (see below).
    currentValue 当前值
        The current element being processed in the array.
    currentIndex
        The index of the current element being processed in the array. Starts at index 0, if an initialValue is provided, and at index 1 otherwise.
    array  当前数组
        The array reduce was called upon.
    initialValue
        累加器初始值，
        如果不存在则accumulator为第一个元素值，currentValue为第二个元素值 currentIndex从1开始
        如果存在则accumulator为初始值，currentValue为第一个元素值 currentIndex从0开始
        [Optional] Value to use as the first argument to the first call of the callback. If no initial value is supplied, the first element in the array will be used. Calling reduce on an empty array without an initial value is an error.
Return value
    The value that results from the reduction.
```
## Array.from介绍
```
Parameters
    arrayLike
        An array-like or iterable object to convert to an array.
    mapFn [Optional]
        Map function to call on every element of the array.
    thisArg [Optional]
        Value to use as this when executing mapFn.
Return value
    A new Array instance.
Array.from可以把类数组对象转变成数组如函数的入参,
document.querySelectorAll("div")返回的nodelist
```


## Object.assign()
```
Object.assign(target, ...sources)

The Object.assign() method is used to copy the values of all enumerable own properties from one or more source objects to a target object. It will return the target object.

Parameters
    target
        The target object.
    sources
        The source object(s).
Return value
    The target object.

被用来复制一个或多个对象的可枚举属性（浅拷贝,需留意多层级嵌套对象）
```

## Number(string)
```
Using the Number object to assign values to numeric variables
如果string不能转换为数字则返回NaN
```

## object literals
```
这就是对象字面量
const a={
    name:zs,
    age:18,
}

const b =[1,3,4,5,6,7];
```

## export 和export default区别
```
导出和默认导出
导出可以有多种但export default只能有一个

function foo(){

}
function bar(){

}

export var named1 = 'Michael';
export var named2 = 'Jackson';
export {bar} ;
export default foo;

```

## import foo, { named1, named2 } from 'foo';
```
即在一条import语句中，同时输入默认方法和其他变量，可以写成下面这样。
import foo, { named1, named2 } from 'foo';
foo为export default
named1 和 named2 为export {named1},eport {named2}
```
