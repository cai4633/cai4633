---
title: http入门
date: 2020-03-07 01:08:51
categories: 前端
tags: http
---

在之前的一段时间的里我着重去学习了html,css,js以及vue等应用类知识点，而忽略了http的学习，导致遇到问题有些力不从心，无法洞悉问题的本质，所以这几天重新学习了一下http。
## www（world wide web）
http出现之前，人们主要通过emailh和ftp的方式来上网交流。1980-1990年间，http和gopher等方案被提出，后来http以其易用性胜出。1990年，Tim Berners-Lee（李爵士）发明了www，主要包括三个概念：**URI,HTTP和HTML**

## URL
URI（uniform resource identity）通用资源标识符包括URN（uniform resource name)和URL(uniform resource location)。
通过URN可以确定唯一的资源，URL可以确定唯一的地址。
### URL的组成
URL通常由协议、主机、端口、路径、查询参数以及锚点等组成。
![url结构](/images/url.png)
`protocol :// hostname[:port] / path / [parameters][?query]#fragment`
**协议**：http，https，[ftp，mailto，ed2k，file:///]
**主机**： `.com 一级域名（顶级域名）  baidu.com 二级域名  www.baidu.com 三级域名
**端口**：对应服务器端口。每个端口的分配功能不同。（默认端口80）

|端口|分配功能|
|:--:|:--:|
|21|ftp|
|53|DNS|
|80|**http**|
|443|**https**|
|1080|代理端口|

**路径**：以‘/’开头的路径，不同于文件系统的路径，它决定于后端的路由。
**查询参数**：请求或查询的键值对
**锚点**：这个部分不会发送给后端，它根据前端页面元素id值来跳转。
### DNS（domain name system)域名系统
我们访问网页，最终都是在访问域名对应**IP地址**的服务器。浏览器中的域名会先在电脑中的**HOST文件**(vi /etc/hosts)中寻找对应的IP，如果找不到就会去电信公司的DNS系统中寻找。很多大公司会有多台服务器，这样同一个域名可能对应多个IP，DNS系统会根据就近原则返回给我们对应的服务器。
查询ip：
```
nslookup <域名>
ping <域名>
```

## http
互联网中包含客户端和服务器。我们在浏览器中上网实际上是：client发送请求→server接受请求→server发送响应→client接收并下载响应。
**bash访问url:**  ` curl [-X <get/post/put/patch/delete>] [-d "12345"] -s -v [-H "a:b"] -- <url>  `
### http请求格式
```
    part1 动词 路径 协议/版本
    part2 Key1: value1
    part2 Key2: value2
    part2 Key3: value3
    part2 Content-Type: application/x-www-form-urlencoded
    part2 Host: www.baidu.com
    part2 User-Agent: curl/7.54.0
    part3 
    part4 要上传的数据
```

1. 请求最多包含四部分，最少包含三部分。（第四部分可以为空）
2. 第一部分请求动词包括 GET（获取/查询） POST(上传) PUT(整体更新) PATCH(局部更新) DELETE(删除) HEAD OPTIONS 等
3. 第一部分的路径包括「查询参数」，但不包括「锚点」（注意区别于url路径）。如果你没有写路径，那么路径默认为 ‘/’
4. 第二部分中的 Content-Type 标注了第 4 部分的格式，第二部分都是key-values。
5. 第三部分永远都是一个回车（\n）。
6. 第四部分是要上传的数据，可以为空。如果上传的数据是汉字，那它会被解析为以“%”分隔的三字节的unicode。

![http请求示例](/images/request.png)

### http响应格式
```
part1 协议/版本号 状态码 状态解释
part2 Key1: value1
part2 Key2: value2
part2 Content-Length: 17931
part2 Content-Type: text/html
part3
part4 要下载的内容
```

1. 第一部分的状态码是服务器对浏览器说的话。
1xx 不常用
2xx 表示请求成功  （200 成功 204 创建成功）
3xx（301 网页永久移走 302 网页暂时移走   304 响应内容与上次一样）
4xx 表示客户端请求出错,一般是404
5xx 表示服务器出错
2. 第二部分中的 Content-Type 标注了第 4 部分的格式，Content-Type 遵循 MIME 规范。
3. 第三部分永远都是一个回车（\n）。
4. 第四部分是要下载的数据。
![http响应示例](/images/response.png)




