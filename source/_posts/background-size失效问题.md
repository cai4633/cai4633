---
title: background-size失效问题
date: 2019-07-09 20:30:27
categories: css
tags: background
---

今天写代码时遇到一个问题，发现background-size属性无效。代码如下：

{% codeblock lang:css %}
	.icon{
		background-size:12px 12px; 
		background: url('assets/img/decrease_1@2x.png');
	    }
{% endcodeblock %}

后来发现原因是，background是一个*组合属性*。它本身包含了background-size属性，默认值是auto。如果把background放在background-size后面，那么background-size会被覆盖为auto，这样就导致background-size设置失效。正确的写法如下：

{% codeblock lang:css %}
	.icon{
		background: url('assets/img/decrease_1@2x.png');
		background-size:12px 12px; 
	    }
{% endcodeblock %}

举一反三，以后遇到其他的*组合属性*一定要注意它出现的*位置*。

