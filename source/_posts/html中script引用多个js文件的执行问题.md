---
title: html中script标签引用多个js文件的执行问题
date: 2019-10-14 22:22:51
categories: js
tags: script标签
---

`<script>`标签可以实现多个js文件的同步引用。如下：
{% codeblock lang:html %}
		<head>
				<script type="text/javascript" src="one.js"></script>
				<script type="text/javascript" src="two.js"></script>
				<script type="text/javascript" src="three.js"></script>
		</head>
{% endcodeblock %}

1. **js文件按顺序加载，执行**；
2. **各个js文件中的全局变量和全局函数可以被其他js文件调用**；
3. **较早执行的js文件不能调用较晚加载的js文件中的变量和函数。例如："two.js"不能调用"three.js"中的变量和函数；**