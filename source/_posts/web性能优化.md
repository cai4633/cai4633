---
title: web性能优化
date: 2021-01-03 09:22:12
categories: 前端
tags: 性能优化
---

![](/images/web_performance_optimization.png)
一个前端开发必定绕不开**性能优化**这个话题。web 性能优化通俗的讲就是让页面尽快呈现在用户眼前。要想解决性能优化这个难题必须提前了解[浏览器从输入 URL 到渲染页面的整个过程](/2020/04/19/浏览器输入URL到页面显示全过程/)。简单的来说 web 性能优化主要从以下几个方面入手：

1. DNS 查询过程
2. HTTP 请求过程
3. 资源下载过程
4. 浏览器渲染过程
5. 缓存过程

## 一、DNS 查询优化

浏览器地址栏输入 URL 发送出去后，域名会通过 DNS 服务器进行域名解析并返回对应的 IP 地址。这个过程通常比较快，但是也可以有优化的空间。

### DNS 缓存

DNS 缓存一般都是浏览器等自动缓存的。但是了解其相应的过程还是必要的。DNS 解析首先查询**浏览器缓存**，然后查询操作系统**HOSTS 文件**，再查询**路由器缓存**，如果无法找到缓存才正式向**本地 DNS 服务器** 或者**根服务器**逐级请求解析。这其中我们可以**更改 hosts** 加快解析速度，但是我们不能指望每个用户都会改 hosts。

### 负载均衡

如果同一台服务器同时链接的用户数过大时，服务器可能会崩溃。解决办法就是 DNS 负载均衡技术。原理是在 DNS 服务器中为同一个域名配置多个 IP 地址,在应答 DNS 查询时,根据服务器的负载量以及离用户的距离等返回不同的解析结果,将客户端的访问引导到不同的服务器上,从而达到负载均衡的目的｡例如 **CDN 节点**。

### DNS 预获取

现代浏览器默认会对页面中和当前域名（正在浏览网页的域名）不在同一个域的域名进行预获取，并且缓存结果，这就是隐式的 DNS Prefetch。如果想对页面中没有出现的域进行预获取，那么就要使用显式的 **dns-prefetch** 了。

```
<!-- 打开dns-prefetch -->
<meta http-equiv="x-dns-prefetch-control" content="on">
<link rel="dns-prefetch" href="//www.baidu.com">
<link rel="dns-prefetch" href="//zhihu.com">
```

```
<!-- 关闭dns-prefetch 禁止隐式dns预获取 -->
<meta http-equiv="x-dns-prefetch-control" content="off">
```

## 二、http 请求优化

当域名解析成功后，浏览器就会和对应的服务器建立 TCP 连接，并发送 http 请求。http 请求可以优化的地方也有很多。

### 减少 http 请求数

能合并的文件**尽量合并**，比如使用雪碧图等都可以有效减少 http 请求数。

### TCP 连接复用 (keep-alive)

HTTP 协议采用“请求-应答”模式。http1.0 时代，客户端每次请求都要和服务器新建一个 TCP 连接，完成之后立即断开连接。而使用 Keep-Alive 模式会使客户端到服务器端的 TCP 连接持续有效。

```
<!-- 启用Keep-Alive -->
Connection: Keep-Alive
```

HTTP 1.1 时代，默认开启 Keep-Alive 模式，只有加入 `Connection: close `才关闭连接。Keep-Alive 模式的属性可以设置 ：

```
<!-- timeout=6 表示这个 TCP 通道可以保持 6 秒，max=50 表示这个长连接最多接收 50 次请求就断开 -->
Keep-Alive: timeout=6, max=50，
```

### 浏览器请求阻塞

浏览器在**同一时刻对相同域名**的请求数有一定的限制，不同的浏览器表现不同，一般都是 4-10 个。chrome 浏览器默认情况下是 6 个。为了绕过这个限制，可以采用**多域名**的方法。

### cookie-free

**浏览器不会把主域名的 Cookie 传给子域名**。利用这一特性我们可以将 css 和 image 单独放在不同的子域名中，减少网络开销，一定程度提高了页面加载速度， 这种技术应用叫做 cookie-free。

## 三、资源下载优化

资源下载速度的快慢取决于文件的大小和服务器带宽，所以优化也是从这两方面入手。

1. 提高服务器 **server 带宽**
2. 开启服务器 **gzip** 压缩

## 四、浏览器渲染优化

浏览器资源渲染过程如下：

1. 解析 HTML 文件，构建 **DOM 树**，与此同时浏览器主进程下载 CSS 文件
2. CSS 文件下载完成，解析 CSS 文件构建 **CSSOM 树**；
3. CSSOM + DOM 合并成 **render 树**；
4. 布局（**Layout/reflow**） render 树 ，负责 render 树中的元素的尺寸和位置等计算；
5. 绘制（**paint**） render 树 ，绘制页面的像素信息（color 等）；
6. 浏览器主进程将图层交给 GPU 进程，GPU 进程将各个**图层合成**（composite），最后显示出页面；

由此可见：

1. css 的加载**不会影响 dom 的解析**，但**会影响 dom 的最终渲染**。
2. 当浏览器遇到 `<script>` 时，会**阻止解析器**继续执行。直到 `CSSOM 构建`完毕，JavaScript 才会运行并继续完成 `DOM 构建`过程。所以解析优先级 `CSSOM > JS > DOM`。

```
<script src='' async>
// async: 浏览器遇到带async的 script 标签时会继续解析 DOM，同时脚本也不会被 CSSOM 阻止，即不会阻止 CRP。

<script src='' defer>
// defer: 脚本需要等到文档解析后（ **DOMContentLoaded 事件前**）执行，而 async 允许脚本在文档解析时位于后台运行（两者下载的过程都不会阻塞 DOM，但执行会）。
// 当脚本不会修改 DOM 或 CSSOM 时，推荐使用 async 。
```

了解以上原理后，我们就可以提出以下几点优化建议：

1. 合并重复的 css 样式，将 css 样式放在文档前部（如：`<head>中`）,将 js 放在文档后部（如：`<body>最后面`）；
2. 只渲染**首屏**，页面采用**懒加载**方式；
3. 有必要的地方采用页面**预加载**；

## 五、缓存优化

1. DNS 缓存 （前面已经提到，此处略）
2. http 缓存
   http 缓存主要缓存的对象是向服务器请求的 **css/js/image** 等文件。其主页入口**html 文件**一般不做缓存。下次请求时，缓存过的文件一般就不会向服务器发送请求。如果想解除缓存限制，只需要修改请求 url **版本**号（`MD5` 算法）。
   添加缓存的方式：
   1. 后端 response header 添加 `cache-control: max-age=1000 `，此方法最常用；
   2. Expires 和 Last-Modified，逐步被弃用；
   ```
   Expires:Thu, 02 Apr 2009 05:14:08 GMT
   Last-Modified:Tue, 24 Feb 2009 08:01:04 GMT
   ```
   3. ETAG，逐步被弃用
      `Etag:“5d8c72a5edda8d6a:3239″`
