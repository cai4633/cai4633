---
title: HTML入门
date: 2020-03-13 01:14:58
categories: html
tags: html
---
HTML全称**Hypertext markup language**，它是整个网页的内容（骨架）。学习HTML就是要掌握好它常用的标签，html标签在网页中又叫html元素。它分为block元素，inline元素和inline-block元素。这些都只是标签默认的属性，实际开发中都可以通过css改变。

## HTML版本
HTML版本主要有：HTML 4.01 → XHTML → HTML 5 → HTML 5.1，由W3C根据浏览器实际情况编写文档。`<!DOCTYPE HTML>`用来声明文档类型。

## HTMl标签
### 常见的html标签
`(html、body、head）、a、form、input、button、h1(h2、h3 )、p、ul、ol、small、strong、div、span、kbd、video、audio、svg。
`这些标签除了div和span，其他标签都有默认样式。
### 空标签
空标签里面不能有内容（子元素），通常在一个空元素上不需要使用闭合标签。例如：
```
<br /> 换行标签，通常用于文本格式换行
<hr/> 水平分割线
<input />  用于为基于Web的表单创建交互式控件，以便接受来自用户的数据。
<link /> 指定了外部资源与当前文档的关系. 这个元素的使用方法包括为导航定义关系框架.这个元素经常用来链接css文件。
<img /> 文档中的图像。
<meta /> 元素表示那些不能由其它HTML元相关元素 (<base>, <link>, <script>, <style> 或 <title>) 之一表示的任何元数据信息. 

<isindex /> 使浏览器显示一个对话框，提示用户输入单行文本。
<area /> 在图片上定义一个热点区域
<param />  定义了 <object>的参数
<col /> 定义表格中的列，并用于定义所有公共单元格上的公共语义。它通常位于`<colgroup>`元素内。
<bgsound /> IE浏览器中设置网页背景音乐的元素。
<wbr /> 一个文本中的位置，其中浏览器可以选择来换行，虽然它的换行规则可能不会在这里换行。
<base /> 指定用于一个文档中包含的所有相对URL的基本URL。
<nextid />  是一个过时的 HTML 元素, 它使下一个 web 设计工具能够为其定位点生成自动名称标签。 它是由该 web 编辑工具自动生成的, 不需要手动调整或输入。这个元素的区别是成为第一个元素, 成为一个 "丢失的标签" 被淘汰的官方公共 DTD 的 HTML 版本。
<basefont /> 用来设置文档的默认字体大小。（目前已废弃 ）
<embed /> 用于表示一个外部应用或交互式内容的集合点，换句话说，就是一个插件。 
<keygen />  为了方便生成密钥材料和提交作为 [HTML form]的一部分的公钥.这种机制被用于设计基于 Web 的证书管理系统。(已废弃)
<plaintext /> 将起始标签后面的任何东西渲染为纯文本，不会解释为 HTML。它没有闭合标签，因为任何后面的东西都会看做纯文本。(已废弃)
<spacer /> 它可以向页面插入间隔。它由 Netscape 设计，用于实现单像素布局图像的相同效果，Web 设计师用它来向页面添加空白，而不需要实际使用图片。（已废弃）
 ```
### 可替换元素
可替换元素（replaced element）的内容不受当前文档的样式的影响，CSS 可以影响可替换元素的位置，但不会影响到可替换元素自身的内容。例如：`<img> <iframe> <video> <embed> `,某些元素仅在特定的情况下是可替换元素，例如：`<option> <audio> <canvas> <object> <applet> <input>`.用 CSS `content` 属性插入的对象是匿名的可替换元素。它们并不存在于 HTML 标记中，因此是“匿名的”。

### 几个重点标签
#### iframe和a标签
```
<iframe src='https://qq.com' name='page1' frameborder='0'>   //嵌套页面 页面默认大小是100*50px
<a href='#' target='_self'>Hello</a>  //跳转标签（get请求）
```
1. a标签的target取值：`_self(defaults) _blank  _parent _top framename`
其中_parent和_top是结合iframe嵌套父子关系来确定的。通过指定target='framename'可以使页面在对应的iframe中显示。

2. 通过a标签下载文件
    1. 通过指定a标签的download属性可以强制文件下载。
    2. 如果不加download，而且http响应的内容浏览器不能展示那么文件也会被下载，例如：content-type:application/octet-stream.

3. a标签herf=''的取值：(最好不要用file协议（file:///)来打开文件。)
    1. ` http(s)://XXX `    http/https协议地址。    （例如 http://qq.com) 浏览器会向服务器发送get请求
    2. `//XXX`        继承当前文件协议。 （例如 //qq.com) 浏览器会向服务器发送get请求
    3. `XXX `          相对路径。 （例如 qq.com) ，浏览器会向服务器发送get请求
    4. `#XXX`     锚点。跳转到当前页面指定的id处，且锚点会添加到url后面，但是浏览器不会向服务器发送请求
    5. `?name=cai`  浏览器会向服务器发送get请求 ,查询字符串会添加到url后面
    6. `javascipt:;` javascript伪协议。

#### form和input标签
form和a标签都可以发起请求。a只能发起get请求，form既可以发get,也可以发post请求。不过form一般用来发post请求，提交表单内容。它们target属性的功能也是一样的。
```
<form action='index.php' method='post' target=''>
    <input type='password' name='XXX' value=''> 
    <input type='checkbox' name='XXX' value=''> 
    <input type='radio' name='XXX' value=''> 
    <select name='hobby' multiple>
        <option value='basketball'>basketball</opiton>
        <option value='baseball' disabled>baseball</opiton>
        <option value='soccer'>soccer</opiton>
    </select>
    <textarea style='resize:none' name='info'></textarea>  
    <input type='submit' name='XXX' value=''> 
    <button>button</button>     当表单中没有提交按钮，那么未设置type的button按钮会自动升级为submit按钮
</form>
```
1. form中的input**必须添加name和value属性**，它们在提交的时候会以键值对的形式发送给服务器。这时候name和id意义不同。
3. input[type='checkbox']和input[type='radio']可以通过name属性给选项分组，radio默认value是'on'.
2. label指定标签的两种用法：
    1. 指定for
        `<label for='check'>性别</label><input id='check' type='checkbox' name='sex' value='man'>man`
    2. label包裹input
    `<label>性别<input id='check' type='checkbox' name='sex' value='man'>man</label>`
4. textarea 默认是可变尺寸的。通过css `resize:none`可以取消默认样式。宽度和高度也最好用css来设置，rows和cols属性不可靠，尤其是cols。
5. input[type=button]和button标签的区别：input是空标签，button不是；而且**未设置type的button可以自动升级为submit**。


#### table标签
```
	<table border="1">
		<colgroup>
			<col bgcolor='red' width='100'>
			<col bgcolor='red' width='100'>
			<col bgcolor='red' width='100'>
			<col bgcolor='red' width='100'>
			<col bgcolor='red' width='100'>
		</colgroup>
		<thead>
			<tr><th></th><th>姓名</th><th>班级</th><th>语文</th><th>数学</th></tr>
		</thead>
		<tbody>
			<tr><th></th><td>小明</td><td>1</td><td>50</td><td>60</td></tr>
			<tr><th></th><td>小红</td><td>2</td><td>58</td><td>49</td></tr>
			<tr><th></th><td>小明</td><td>1</td><td>50</td><td>60</td></tr>
			<tr><th></th><td>小红</td><td>2</td><td>58</td><td>49</td></tr>
		</tbody>
		<tfoot>
			<tr><th>平均分</th><td></td><td></td><td>50</td><td>80</td></tr>
		</tfoot>
	</table>    
```
1. 以上是table的完整结构，其中colgroup、thead、tbody和tfoot的顺序可以打乱，浏览器会自动调整好顺序thead→tbody→ tfoot。
2. 如果不加thead、tbody和tfoot这些标签，那么浏览器会自动把内容放入tbody中。
3. colgroup可以设置table的部分样式。css中`border-collapse:collapse`可以设置table的border样式。