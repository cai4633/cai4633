---
title: vue项目中遇到的问题
date: 2019-07-02 22:04:58
categories: 前端
tags: vue
---

## 利用v-if来避免异步调用json数据延迟报错；

**_代码如下：_**

{% codeblock lang:html %}
	<template>
	    <div class='header'>
	        <div class="support">
	            <span class="text">
	            	  {% raw %}{{seller.supports[0].description}}{% endraw %}
	            </span>
	        </div>
	    </div>
	</template>
{% endcodeblock %}

*seller*是在vue生命周期函数create()异步获取的，代码如下：

{% codeblock lang:js %} 
	created:function{
	  this.$http.get('/api/seller').then((response) => {
	    let res = response.body;
	    console.log(res);
	    if(res.errno === ERR_OK){
	      this.seller = res.data;
	    }
	  });
	}
{% endcodeblock %}

如果网络有延迟，当dom开始渲染时，seller还没有返回数据，浏览器就会报错。如下图所示：
![浏览器报错](/images/Snipaste1.png)

***解决办法：***
通过添加v-if来限定dom渲染的时机。添加后代码如下：
{% codeblock lang:html %}
	<template>
	    <div class='header'>
	        <div class="support" v-if='seller.supports'>
	            <span class="text">
	            	  {% raw %}{{seller.supports[0].description}}{% endraw %}
	            </span>
	        </div>
	    </div>
	</template>
{% endcodeblock %}

---


## 在less中如何实现字符串拼接
### 如何引入外部less文件（or css文件)
引入css文件一般有两种方法：*@import url()*和*link*
@import url()中url可以直接省略,例如：

{% codeblock lang:css %}
	<style lang="less" scoped="">
		/*one way*/
		@import url('../index.less');

		/* the other way*/
		@import "../index.less";
	</style>
{% endcodeblock %}

但是，省略url时，@import后面路径必须用*双引号*，在vue component中，使用单引号可能导致引用css失败。

{% codeblock lang:html %} 
<style lang="less" scoped="">
	<!-- 在vue中这种方式可能导致引用css失败 -->
	@import '../index.less';  
</style>
{% endcodeblock %}

### 在less中如何实现字符串拼接
在less中，不可以类似js用‘+’表示字符串拼接。需要利用less中的变量的*可变插值*来拼接字符串；

{% codeblock lang:css %}
.bg(@url){
	@bg_url:'~@/assets/img/@{url}@2x.png';
}
{% endcodeblock %}

这其中@{url}就是可变插值的用法。less变量的可变插值可以用在*选择器名称，属性名称，URL和@import语句*这些地方。

### error: ~/.vuerc may be outdated. Please delete it and re-run vue-cli in manual mode
最近，尝试使用@vue/cli@4.0.5来构建项目，但是编译老是出错。不得已换回了@3.0版本。降低版本后，vue create <new project>会报错。
![vue error](/images/vueError2.png)
**解决办法：**
windows系统中'C:\Users\Administrator'里面有个.vuerc 文件。手动删除后，重新创建项目，问题解决。