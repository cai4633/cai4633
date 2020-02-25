---
title: width百分比小结
date: 2020-02-22 20:18:24
categories: css
tags: width
---

width:100%中百分比肯定是相对于父元素而言的，但是由于padding，box-sizing等属性的影响，有时候会迷惑我们。所以今天这个问题小结一下。
## box-sizing
box-sixing属性主要有content-box和border-box两个值，默认是content-box。这个属性直接影响元素本身的width值，间接影响子元素width值。
{% codeblock lang:css %}
    box-sizing: content-box    //当前元素width值只包括content宽度
    box-sizing: border-box    //当前元素width值包括content,padding以及border的值
{% endcodeblock %}

## width:100%的元素是绝对定位
1. absolute绝对定位元素的父元素是非static定位元素。它的width:100%取值等于父元素的content值加上padding值(与border-width无关)。
2. fixed固定定位元素的父元素是body（即浏览器窗口）。它是特殊的绝对定位，fixed固定定位元素的width:100%取值等于父元素（body）的content值和padding值以及border值之和。(与border-width有关)。
总的来说，**fixed固定定位元素的width:100%等于浏览器窗口的width。**

## width:100%的元素是非绝对定位
非绝对定位元素width:100%的取值等于父元素的content值。包括position:relative相对定位元素也符合这一规则。

**浏览器url中出现“：”容易报错，所以width：100%不允许出现在标题中。**

## 小结
width百分比的计算要注意两点：
1. 找准父元素。
    1. 普通文本流和relative相对定位元素以及static定位元素的父元素都是普通的父元素。
    2. absolute绝对定位元素的父元素是离它最近的***非static定位元素***。
    3. fixed固定定位元素的父元素是浏览器窗口（近似于body)。
2. 找准width计算规则
    1. absolute绝对定位元素的width:100%取值等于父元素的content值加上padding值(与border-width无关)。
    2. fixed固定定位元素的width:100%等于浏览器窗口的width。
    3. 除此之外所有元素的width:100%等于父元素的content值。
上面两点可以确定好width值，然后元素本身会根据box-sizing属性来分配content，border以及padding。（margin-box浏览器支持有限）。