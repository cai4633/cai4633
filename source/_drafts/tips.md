---
title: 填坑
date: 2020-02-27 12:53:18
categories:
tags:
---

### sublime text 

- 在sublime_Text中，'Esc'键会导致编辑器进入CommandMode，这时光标和输入会出现变化。可以屏蔽vim模式,在setting中添加：

```
	"ignored_packages":
	[
	"Vintage"
	]
```
---

### ?sublime text中markdown文档如果出现以下代码：
```
{% codeblock lang:css %}
	.icon{
		background-size:12px 12px; 
		background: url('~@/assets/img/decrease_1@2x.png');
	    }
{% endcodeblock %}
```

那么会出现一行pink line，原因是"~@"导致的。
那么具体的原因是什么？



### 如何使背景图片显示模糊效果？

### 添加的点击事件无效，可能是由于z-index设置导致你点击的不是你想点击的元素。

### vertical-align：middle 如何完美垂直居中：
	middle居中：基于基线baseline上移1/2倍x-height的位置与元素中线对齐；
	所以完美垂直居中与font-family ， font-size， line-height有关。

### border-left不起作用的解决办法：
	1. 给元素添加 box-sizing:border-box 属性，让元素变成一个模型框之后就会有效果。
	2. 取消元素的width:100%属性。

### position:absolute中top,bottom,left,right会影响没有设置width和height的元素的大小；

### sublimetext中查看文件scope_name的快捷键是ctrl+shift+alt+p;

### 图片宽高相同且自适应宽度的两种方法：
1. 元素padding-top：100%，height:0px，然后设置background-image
2. 父元素padding-top：100%，height:0px，position:relative,子元素img position：absolute,width:100%,height:100%

### padding可以在不增加元素大小的情况下，增大点击区域。

### vue自定义事件$emit(),监听事件$on, 自定义事件不能冒泡。
	v-on:用在普通元素上时，只能监听原生 DOM 事件。用在组件上时，只能监听子组件触发的自定义事件，需要监听原生dom事件需要加上.native

	v-on:在监听原生 DOM 事件时，方法以事件为唯一的参数。如果使用内联语句，语句可以访问一个 $event 属性：v-on:click="handle('ok', $event)"。

### ?tanslate(百分比基准)

### 组件的显示和隐藏一般放在组件内部执行比较好。
### better-scroll
   1.  wrapper内部只能有一个子元素。
   2. 只有content的高度大于wrapper高度时候，才可以滚动。
   3. 要注意初始化时机this.$nextTick(new BS(dom,options););
   4. 要直接或间接设置wrapper高度；
   5.  	const options = {
	 	  pullUpLoad: true,
	 	  scrollbar: true,
	 	  pullDownRefresh: true,
	 	  probeType: 3,
	 	  click: true
 	};
 	6.      this.foodsScroll.on('scroll', (pos) => {
		        this.scrollY = Math.abs(Math.round(pos.y));
	      });
