---
title: js全局对象和原型链
date: 2020-03-29 11:58:21
categories: js
tags: ['js','原型链']
---
## 全局对象global(浏览器window)
ECMAScript 规定全局对象叫做 global，但是浏览器把 window 作为全局对象。window 的属性就是全局变量。其中的全局变量分为两种：
1. ECMAScript规定的，例如：
```
global.parseInt()
global.parseFloat()
global.Number
global.String
global.Boolean
global.Object
```
---
2. 浏览器自己规定的，例如：
```
window.alert()
window.prompt()
window.comfirm()
window.console.log()
window.console.dir()
window.document
window.document.createElement()
window.document.getElementById()
```
---
### Number
```
let n = 11          //基本number数据类型
let n = Number('11');   //显式转化成number类型
let n = new Number(11)  //创建number对象，有toString()和valueOf()方法
11.toString()   //11本身没有toString()方法，当调用toString()时，11会临时转化成Number对象，当toString()调用结束，11会再次变成基本number类型（隐式转换）
```

### String
```
let n = '22'         //基本string数据类型
let n = String('11');   //显式转化成string类型
let n = new String(11)  //创建string对象，有toString()和valueOf()方法
```

### Boolean
```
let n = true         //基本Boolean数据类型
let n = Boolean('11');   //显式转化成Boolean类型
let n = new Boolean(11)  //创建Boolean对象，有toString()和valueOf()方法
```
---
## __proto__ 和 prototype
所有的对象都有toString()和valueOf()方法，js将这些公用属性都放在`__proto__`对象中。而为了防止`__proto__`对象因没被引用而被垃圾回收，所以就有了`Object.prototype`。`__proto__`是存在于对象实例中，prototype是存在于构造函数中。
```
对象实例.__proto__ === 构造函数.protoType
let obj = new Object()   //obj.__proto__ === Object.prototype
```
### 对象原型链关系图：
![js对象与原型链](/images/proto.png)

---

### 几个难懂的原型链
```
Object.prototype.__proto__ ==== null
Function.__proto__ === Function.prototype
Object.__proto__ === Function.prototype
```
