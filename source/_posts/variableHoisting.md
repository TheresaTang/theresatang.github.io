---
title: variableHoisting
date: 2019-06-29 11:48:33
tags:
---
产生原因：在JavaScript中，包括变量和函数在内的所有声明都会在任何代码被执行前首先处理，即变量定义会在编译阶段进行，而变量赋值是留着原地等待被执行的。即：
```javascript
console.log(a)  // undefind
var a = 2
//实际上会被处理成如下
var a
console.log(a)
a = 2
```
注意：对于let、const声明的变量不存在变量提升，必须先定义再使用
声明提升有两种情况需要注意：
### 函数声明会被提升，但是函数表达式不会
原因：变量表示符被提升，但是并没有赋值，即使是具名的函数表达式，名称标示符在赋值之前也无法在作用域中使用
```javascript
foo () // TypeError: foo is not a function
bar() // ReferenceError：bar is not defined
var foo = function bar () {
 //函数体
}

// 实际上JavaScript会把上述代码处理成以下
var foo
foo () // TypeError
bar() // ReferenceError
foo = function bar () {
  var bar = ...self...
 //函数体
}
```
### 函数优先
函数声明和变量声明都会被提升，但是函数会首先被提升，然后才是变量，如下所示
```javascript
foo () // 1
var foo
function foo () {
 console.log(1)
}
foo = function bar () {
 console.log(2)
}
 //实际上JavaScript会把上述代码处理成以下, 变量foo声明被覆盖，但是后面的函数声明可以覆盖前面的
function foo () {
 console.log(1)
}
foo () // 1
foo = function bar () {
 console.log(2)
}
```
另外，一个普通块内的函数声明，通常会被提升到所在作用域的顶部，如下所示
```javascript
foo() // b
var a = true
if () {
  function foo () {
    console.log(a)
  }
} else {
  function foo () {
    console.log(b)
  }
}
```
来源参考：《你不知道的JavaScript 上卷》