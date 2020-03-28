---
title: inline、inline-block和block元素的区别
date: 2020-02-23 03:46:56
categories:
tags:
---
常见的元素有block块元素,inline内联元素和inline-block元素。
## block元素特点
1. 宽度默认auto,**block元素width:auto时，宽度会尽可能宽。**（默认填满父元素）,单个block元素独占一行，**即使宽度设为0px**。
2. 宽度和高度可以设置。
3. padding、margin、border都可以设置。
4. 内部可以嵌套block、inline和inline-block等各种元素。

## inline元素的特点
1. 不会独占一行，多个inline元素可以占据同一行，直到宽度超过父元素才会另一行。
2. 宽度和高度不可以设置，默认为auto，由内容宽高决定，具有包裹性。
3. **padding、margin和border在水平方向上可以设置，垂直方向上（padding-top|padding-bottom|margin-top |margin-bottom|border-top|border-bottom）设置无效**。border-top和border-bottom即使在视觉上有效果，但实际上布局上不起作用。 
4. inline元素内部理论上不可以嵌套block元素

{% iframe /pages/demo4 100% 400px %}

## inline-block元素特点
inline-block同时具备inline和block元素的特点。
1. 不独占一行
2. 宽高可以设置，当宽高设为auto时又具备包裹性
3. padding、margin、border都可以设置
4. 内部理论上不可以嵌套block元素

## 小结 
1. inline和inline-block元素**第三条**特点的差别很容易让人忽略。
2. inline元素由于内容而自动换行后，会导致border破裂。
