---
title: host文件的作用和用法
date: 2019-10-26 20:49:11
categories: 服务端
tags: ["host", "Apache", "vHost"]
---

## Hosts 文件的工作方式

Window 系统中有个 Hosts 文件（没有后缀名）。
我们访问网站时，首先要通过**DNS 服务器**把要访问的网络域名（XXX.com）解析成 IP 地址后，计算机才能对这个网络域名进行访问。如果对于每个域名请求我们都要等待 DNS 服务器解析并返回 IP
信息，这样访问的效率就会降低。为了提高对经常访问的网络域名的解析效率，可以通过利用 Hosts 文件中建立域名和 IP 的映射关系来达到目的。
根据 Windows 系统规定，在进行 DNS 请求以前，Windows 系统会先检查自己的 Hosts 文件中是否有这个网络域名映射关系。如果有则，调用这个 IP 地址映射，如果没有，再向已知的 DNS 服务器提出域名解析。也就是说**_Hosts 的请求级别比 DNS 高_**。

---

## Hosts 文件的语法

Host 规定每段只能包括一个映射关系，也就是一个 IP 地址和一个与之有映射关系的主机名。
IP 地址要放在每段的最前面，映射的 Host name(主机名)在 IP 后面，中间用空格分隔。注释可以以“#”分割后的文字来说明。

```
grammar:
       ip host-name
127.0.0.1 localhost
```

---

## Hosts 文件的用法

1. 加快域名解析
   对于要经常访问的网站，我们可以在 Hosts 中配置域名和 IP 的映射关系，提高域名解析速度。由于有了映射关系，当我们输入域名计算机就能很快解析出 IP，而不用请求网络上的 DNS 服务器。

2. 方便局域网用户
   在很多单位的局域网中，会有服务器提供给用户使用。局域网中一般很少架设 DNS 服务器，访问这些服务器时，要输入难记的 IP 地址。这对不少人来说相当麻烦。现在可以分别给这些服务器取个容易记住的名字，然后在 Hosts 中建立 IP 映射，这样以后访问的时候，只要输入这个服务器的名字就行了。

3. 屏蔽网站
   现在有很多恶意网站。对于这些网站我们可以利用 Hosts 把该网站的域名映射到错误的 IP 或本地计算机的 IP，这样就可以达到屏蔽的目的。在 WINDOWS 系统中，约定 127.0.0.1 为本地计算机的 IP 地址, 0.0.0.0 是错误的 IP 地址。
   例如下面的代码：

```
127.0.0.1  # 要屏蔽的网站A
0.0.0.0  # 要屏蔽的网站B
```

4. 顺利连接系统
   对于 Lotus 的服务器和一些数据库服务器，在访问时如果直接输入 IP 地址是不能访问的，只能输入服务器名才能访问。如果我们配置好 Hosts 文件，这样输入服务器名就能顺利连接了。

---

## Apache 中 vhost 的作用

vhost 允许在同一个服务器中（或者计算机、主机）配置多个域名和端口。方法如下：

1. 在 apache 配置文件 httpd.conf 中，新增端口 5000：

```
#Listen 12.34.56.78:80
Listen 80
Listen 5000
```

2. 在 D:\xampp\apache\conf\extra\httpd-vhosts.conf 中，新增端口解析：

```
<VirtualHost *:5000>
    ServerAdmin webmaster@tp5.com
    DocumentRoot "D:/XAMPP/htdocs/tp5/public"
    DocumentRoot "D:/XAMPP/htdocs/tp5/public"
    ServerName tp5.com
    ErrorLog "D:/XAMPP/htdocs/tp5/public/access.log"
    CustomLog "D:/XAMPP/htdocs/tp5/public/access.log" common
</VirtualHost>
```

### vhost 配置语法

1. <VirtualHost [ip:端口]> 指定虚拟主机所使用的 IP 地址，端口或域名（如果 web 服务器上有多个 IP，就可以制定某个 IP 的某个端口是哪个主机）
2. ServerAdmin 管理员邮箱（Apache 本地服务器中一般设成 webmaster@[绑定的域名]）
3. DocumentRoot 网站根目录（地址两端加引号,必填）
4. ServerName 绑定的域名 （必填）
5. ServerAlias 要绑定的虚拟主机的别名。（选填，如有多个域名，中间以空格分隔。支持*，?两种通配符，比如 *.abc.com，表示任意一个 abc.com 的二级域名都可访问。）
6. ErrorLog 错误日志目录 （选填）
7. CustomLog 用户日志目录 （选填）
   **_Apache 在接受到请求时,如果没有匹配到对应的 ip 端口和绑定域名，就会默认第一个 VirtualHost 起作用。_**

---

## host 和 vhost 的区别

如图：
![简单流程图](/images/host1.png)

host 是本地计算机中**_通过请求域名来解析 ip_**的文件，vhost 是 web 服务器中通过域名和 ip 端口来确**_定 web 服务器分发内容的目录_**的文件。
