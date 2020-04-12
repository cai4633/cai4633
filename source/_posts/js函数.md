---
title: js函数
date: 2020-04-06 17:22:06
categories: js
tags: ['js','function']
---
<img src='/images/js_function.png'>

## js函数的5种申明方式：
函数一定有一个返回值，即使我们申明时没有return,js会自己添加`return undefined`。
1. 具名函数
`function add(x,y){return x+y}    //add.name === 'add' `
2. 具名函数表达式
`let fn = function add(x,y){return x+y}   //fn.name ==='add' `
3. 匿名函数表达式
`let fn = function (x,y){return x+y}    //fn.name === 'fn'`
4. new Function()
`let fn = new Function('x','y','return x+y')   //fn.name === 'anonymous'`
5. ES6箭头函数（只能匿名）
`let fn = (x,y) => {return x+y}   //fn.name === 'fn'`

方法1和方法2申明的区别：
1. 方法1`变量add`有申明提升的效果而方法2中`变量add`不会变量提升。
2. 方法1中`add()`在add函数内部和外部都可以被调用，但方法2中`add()`只能在add函数内部被调用。

## 函数的调用
1. `fn(param1,param2)`
4. `obj.fn()`
2. `fn.call(undefined/null,param1,param2)`
3. `fn.apply(thisArg, [argsArray])`

## this和arguments
`函数调用：fn.call(this,arg1,arg2,...)`
其中，`this`就是第一个参数；
`arguments`是除第一个参数外其他参数组成的的伪数组。
### this
`this`设计目的就是在函数体内部，指代函数当前的*运行环境*。`this`在**函数调用**时才被确定。它的本质是fn.call(context,arg)传入的第一个参数，一般来说`传入的context`是个对象。非严格模式下，当传入undefined/null时，this指代window全局对象；严格模式下("use strict"),传入undefined/null或者任何数据类型，this值都直接指代传入的值。实现的结果如下：
1. `fn(param1,param2)` this指向window
2. `obj.fn()` this指向obj（通过对象方法调用时，this指向最后调用函数的对象）
3. 箭头函数内部this跟函数外部this一致
4. `new fn()`中的this指向被创建的新对象本身


## 词法作用域
作用域就是js**变量的可访问范围**，它决定了变量的可见性和生命周期。函数在**定义时**，就已经决定了它的作用域。作用域分为全局作用域、局部（函数）作用域和块级作用域（let/const)。作用域可以嵌套，嵌套内部作用域可以访问嵌套外部的变量，这样就形成了**作用域链**。
作用域链是**变量**的可访问范围。
原型链是**对象属性**的可访问范围。
### 难点
作用域在**函数定义时**就已经确定。但是此时确定的只是**变量的可访问范围**，变量的**值并不会确定不变**，所以**函数调用时变量的值由调用时决定**。

## 闭包
**函数和lexical environment词法环境（包括作用域链）**捆绑在一起构成闭包（closure）。也就是说，闭包可以让你从内部函数访问外部函数作用域。在 JavaScript 中，每当函数被创建，就会在函数生成时生成闭包。所以，我们看到有人说一个函数return一个函数就是闭包的说法是错误的，这只是在利用闭包而已。我们**在返回一个函数的同时也返回了这个函数的词法环境（作用域链）**，这样我们就可以在函数外部访问到函数内部的变量了。

## callStack
js函数调用时会被压入callstack中，标记好，等待函数执行完成(return;)时，会再把函数弹出callstack中。
