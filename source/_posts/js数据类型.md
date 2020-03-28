---
title: js数据类型
date: 2020-03-28 01:40:09
categories: js
tags: js
---
## js简史
1991年，Tim Berners-Lee发明www万维网 
1992年，Tim Berners-Lee 及其同事发明了css
1993年， w3c万维网联盟出现
1995年，Netscape公司推出了navigator浏览器，js之父Brendan Eich用十天时间开发出了js（mocha）。之后，Unicode发布了utf-8
1996年，Microsoft发布了IE浏览器和JScript
Netscape开源firefox浏览器，由Mozilla委员会维护。向ECMA(欧洲计算机制造商协会)申报标准并起名ECMAScript.
2004年 ， Microsoft发布IE5.5，使js可以发送http请求
Gmail的出现，让人们认识到js不仅仅是脚本玩具语言，也可以成为真正的编程语言
2010年，front-end行业出现

ES3 :标准库少  → ES5：IE7不支持 → ES6：IE8不支持
ES6借鉴了rails社区coffeeScript的很多特性：类、箭头函数等，集大家之所长
后来es每年一更新

## js数据类型
js有七种数据类型
基本类型:**number,string,boolean,symbol,null,undefined**
复杂类型：**object**(包括array,function,date等)

## number
js中实际上没有整数和小数之分，所有的数字均以**64位浮点**数存储。 它的表示方法如下：
```
十进制表示法：0.1  .1   1e-1 
二进制表示法：0b0101010     #以0b开头
八进制表示法：0o0101010     #以0开头，ES5新添以0o开头
十六进制表示法：0x0101010   #以0x开头
```
### 类型转换
1. Number(anything)
2. parseInt(str)
3. parseFloat(str)
4. anything - 0

## string
空字符串：''    （区别于空格字符串'   ')
多行字符串：
1. 借鉴于bash中语法：
```
'aaaa\
bbbb'               # 结果为"aaaa↵bbbb"   (不推荐使用)
```

2. ` 'aaaa' + 'bbbb'              # 结果为aaaabbbb   (推荐使用)`
3. es6新语法：
```
`aaaa
bbbb                # 结果为"aaaa↵bbbb"   (推荐使用)
`
```

### base64
Base64就是一种基于64个可打印字符(52个大小写字母、10个数字以及+、/h和=)来表示二进制数据的方法。
```
window.btoa(str)  创建一个base64编码的字符串
window.atob(str)  解码base64编码的字符串
```
如果str中包含汉字，上面的方法会报错。这时候需要使用：
```
window.btoa(encodeURIComponent(str))  创建包含非ASCII(如汉字)的str的base64
decodeURI(window.atob(str))  解码base64
```

### 类型转换
1. String(anything)
2. toString(anything)
3. ''+anything

## boolean
boolean只有true和false。
a && b : 只有两个都为真时，表达式结果为真
a || b ：只有两个都为假时，表达式结果为假
1. 5个false值：'' ,null,undefined,0,NaN
2. 所有的对象都是true

### 类型转换
1. Boolean(anything) 
2. !!anything 

## symbol
Symbol()用来生成一个**全局唯一**的值。

## null和undefined
它们都表示没有值。
1. (规范)一个变量没有被赋值，那么这个变量的值就是 undefined
2. (习惯)如果想表示一个还没赋值的对象，就用 null。如果想表示一个还没赋值的字符串/数字/布尔/symbol，就用 undefined（但是实际上直接 var xxx 一下就行了，不用写 var xxx = undefined）

## object
object是基本类型数据无序组合的一种hashTable。
1. object 的 key 一律是字符串，不存在其他类型的 key
2. object[''] 是合法的
3. object['key'] 可以写作 object.key
4. object的key不加''时，只能是合法的标识符（满足以字母、_、$开头的字符串）或者纯数字，object的key加上''时，可以是任意的unicode字符串
5. 删除对象key：delete object.key 。删除后object.key==undefined && key in object ==false
6. 遍历对象key：for(key in object) {code block...}
7. 获取对象所有的key： Object.keys(obj)
7. 获取对象所有的value： Object.keys(obj)

## typeof
typeof是一个判断变量或者表达式数据类型的**操作符**。使用方法：typeof <variable>或者 typeof(variable)
```
    //正常结果
    typeof 111) === 'number'
    typeof '111' === 'string'
    typeof true === 'boolean'
    typeof Symbol() === 'symbol'
    typeof undefined === 'undefined'
    typeof {} === 'object'

    //非常规结果
    typeof null === 'object'
    typeof [] === 'object'      
    typeof function(){} === 'function'    #并没有function类型
    
```
### Object.prototype.toString.call()
由于typeof存在一些bug，所以准确判断一个变量的类型可以用**Object.prototype.toString.call(<variable>)**。
```
    Object.prototype.toString.call(111) === '[object,Number]'
    Object.prototype.toString.call('111') === '[object,String]'
    Object.prototype.toString.call(true) === '[object,Boolean]'
    Object.prototype.toString.call(Symbol()) === '[object,Symbol]'
    Object.prototype.toString.call(undefined) === '[object,Undefined]'
    Object.prototype.toString.call({}) === '[object,Object]'
    Object.prototype.toString.call(null) === '[object,Null]'
    Object.prototype.toString.call([]) === '[object,Array]'      
    Object.prototype.toString.call(function(){}) === '[object,Function]'    #并没有function类型
    
```
