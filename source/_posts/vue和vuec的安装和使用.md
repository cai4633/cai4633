---
title: vue和vuec的安装和使用
date: 2019-10-21 21:00:25
categories: 前端
tags: [vue,vuec]

---
## 安装vue
这段时间，我在学习vue的使用。安装vue有三种方式：
1. CDN方式：直接使用 `<script>` 标签引用vue；
{% codeblock lang:html %}
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>  //引用最新版本vue
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.0"></script>        //引用具体版本vue
{% endcodeblock %} 

2. npm安装：
{% codeblock lang:js %}
   npm install vue  //安装最新稳定版
   npm install -g cnpm --registry=https://registry.npm.taobao.org //可以安装npm淘宝镜像，提高安装速度 
{% endcodeblock %}

3. 安装vue-cli构建工具：
   a. 卸载旧版本vuec
{% codeblock lang:js %}
    npm uninstall vue-cli -g  //vuec 2.0前称作vue-cli, 3.0+后称作@vue/cli
{% endcodeblock %}
    
   b. 安装新版本vuec
{% codeblock lang:js %}
    npm install @vue/cli -g 
{% endcodeblock %}   
<br>
---
## vuec使用
1. 新建vue项目
{% codeblock lang:js %}
    vue create project_name
//  or 
//  vue ui     进入vue gui管理界面
{% endcodeblock %}
项目名称不能出现**大写字母或者驼峰形式**。

2. 进入安装目录，并运行。
{% codeblock lang:js %}
    cd project_name
    npm install
    npm run serve
{% endcodeblock %} 
<br>
---
## element-ui的安装及使用
1. npm安装使用
{% codeblock lang:js %}
    npm i element-ui -S
{% endcodeblock %}
在vuec创建项目中的main.js文件中写入以下代码：
{% codeblock lang:js %}
    import Vue from 'vue';
    import ElementUI from 'element-ui';
    import 'element-ui/lib/theme-chalk/index.css';
    import App from './App.vue';
    
    Vue.use(ElementUI);
    
    new Vue({
      el: '#app',
      render: h => h(App)
    });       
{% endcodeblock %}

2. CDN引用
{% codeblock lang:html %}
<!-- 引入样式 -->
<link rel="stylesheet" href="https://unpkg.com/element-ui/lib/theme-chalk/index.css">
<!-- 引入组件库 -->
<script src="https://unpkg.com/element-ui/lib/index.js"></script>
{% endcodeblock %}

3. 安装vue-cli-plugin-element
这个方法要求@vue/cli 3.0+ 。安装的代码如下：
{% codeblock lang:npm %}
    vue create <my-app>
    cd <my-app>
    vue add element
{% endcodeblock %}   
然后根据提示，插件会自动导入element-ui。

---

## axios的安装
1. 通用的安装方法：
`npm install axios`
2. vue-axios的安装和用法：
`npm install --save axios vue-axios `
**在main.js中引入**
```
   import Vue from 'vue'
   import axios from 'axios'
   import VueAxios from 'vue-axios'
   
   Vue.use(VueAxios, axios)
```
**你可以这样使用它：**
```
Vue.axios.get(api).then((response) => {
  console.log(response.data)
})

this.axios.get(api).then((response) => {
  console.log(response.data)
})

this.$http.get(api).then((response) => {
  console.log(response.data)
})
```

      