---
title: 认识DOM
date: 2020-04-11 20:42:50
categories: js
tags: DOM
---
<img src='/images/dom.png'>

## DOM
### MDN解释
**文档对象模型(DOM)** 将Document(HTML)与脚本（JavaScript）连接起来。但将HTML、SVG 或 XML 文档建模为对象并不是 JavaScript 语言的一部分。**ECMAScript标准和DOM标准是不同机构（W3C和ECMA）维护的不同的标准**。 DOM模型用一个**逻辑树**来表示一个文档，树的每个分支的终点都是一个节点(node)，每个节点都包含着对象(objects)。DOM的方法(methods)让我们可以改变文档的结构、样式或者内容。
### 理解DOM
浏览器将Document文档通过构造函数构建成Object对象，形成整个逻辑树，就是DOM。在JS中，所有的对象都继承于**Object**。在DOM中，所有的构造函数都继承于**Node函数**。DOM主要有四大接口：Node，Document，ElementText（还有一些不常见的）。

## Node常见的属性和方法
### 属性
```
childNodes,firstChild,lastChild,
nextSibling,previousSibling     //有可能返回文本节点
nodeName,nodeType,nodeValue,
outerText,innerText,textContent
ownerDocument,parentElement,parentNode
```
### 方法
```
appendChild()
cloneNode()     // true代表深拷贝且false代表浅拷贝（default）
contains()
hasChildNodes()
insertBefore()
isEqualNode()   //内容一致的node时，返回值是true
isSameNode()    //同一个node时，返回值是true
removeChild()
replaceChild()
normalize() // 常规化
```

## Document常见的属性和方法
### 属性
```
documentElement, body, links, images forms, scripts, title, head,
characterSet,  doctype,  URL, styleSheets,
childElementCount, children
scrollingElement fullscreen hidden location
onxxxxxx origin plugins readyState 
visibilityState
referrer    //返回跳转到当前页面 的**页面的URI**
domain      //document.domain 获取/设置当前文档的原始域部分
```
### 方法
```
close()
createDocumentFragment()
createElement()
createTextNode()
execCommand()
exitFullscreen()
getElementById()
getElementsByClassName()
getElementsByName()
getElementsByTagName()
getSelection()
hasFocus()
open()
querySelector()
querySelectorAll()
write()
writeln()   // 向文档中写入一串文本，并紧跟着一个换行符。
```
## Element和Text的API（见MDN）

## 几个需要注意的属性和方法
1. nextSibling和previousSibling: 有可能返回文本节点
2. nodeName: 几乎所有的的标签nodeName都是大写，比如‘DIV',但是**docu0ment的nodeName是“#document”，svg的nodeName是svg**
3. nodeType: element对应1,text对应3
4. Node.normalize() 方法将当前节点和它的后代节点”规范化“（normalized）。在一个"规范化"后的DOM树中，不存在一个空的文本节点，或者两个相邻的文本节点。“空的文本节点”并不包括空白字符(空格，换行等)构成的文本节点。
ps：两个以上相邻文本节点的产生原因包括：
    1. 通过脚本调用有关的DOM接口进行了文本节点的插入和分割等。
    2. HTML中超长的文本节点会被浏览器自动分割为多个相邻文本节点。
5. innerText和textContent的区别
![innerText和textContent的区别](/images/differenceInText.png)
6. html、document和documentElement的联系
![html、document和documentElement](/images/html.png)
7. document.write()和writeln()
这两个方法可以在DOM上写入内容。但是要注意document.open()后才可以document.write(),write后执行document.close()。如果此后还需要write内容，那么重新open document会导致DOM内容被覆盖。所以document.write()要**谨慎与定时器**一起使用。
