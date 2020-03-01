---
title: hexo的使用
date: 2019-10-24 20:48:26
categories: 前端
tags: hexo
---
## 安装
hexo是一个基于node.js的博客框架。安装hexo，需要提前安装下列application:

1. node.js(version不低于8.6，建议使用10.0+版本)； 
2. git （用于部署博客至线上）

上述应用安装完成后，便可以通过npm安装hexo：
{% codeblock lang:npm %}
    npm install hexo-cli -g
{% endcodeblock %}

## hexo常用命令
### 1. hexo init <new folder>
在指定的<new folder>文件夹中新建一个hexo项目。如果省略<new folder>，便会在当前文件夹先建一个项目。
"hexo init"常常搭配下面的代码来初始化项目：
{% codeblock lang:npm %}
    hexo init <folder>
    cd <folder>
    npm install
{% endcodeblock %} 

### 2. hexo new [layout](title)
这个命令可以新建一个[post,page,draft]。title是文件名。
**新建的draft草稿不会被渲染，用于存储未完成的文章。**当需要将草稿变成文章时，可以手动将.md直接移动到_posts文件夹中，也可以使用“hexo publish [type](文件名(不带.md))”指令。手动将.md移动到_drafts文件夹可以使文章变成草稿。

### 3. hexo genetate
将markdown文本生成静态文件,可简写为hexo g 。
{% codeblock lang:npm %}
    hexo g -d       //生成静态文件后立即部署
    hexo g -f       //清空public文件夹，重新生成静态文件
{% endcodeblock %}

### 4. hexo server
启动服务器,可简写为 hexo s 。可以通过http://localhost:4000/访问。
{% codeblock lang:npm %}
    hexo s -p   //重设服务器端口
    hexo s -s   //只使用静态文件
{% endcodeblock %}

### 5. hexo deploy
部署博客网站，可简写为hexo d 。
{% codeblock lang:npm %}
    hexo d -g   //先生成静态文件，再部署网站
{% endcodeblock %}

### 6. hexo clean 
清除缓存文件 (db.json) 和已生成的静态文件 (public)。
在某些情况（尤其是更换主题后），如果发现对站点的更改无论如何也**不生效**，可能需要运行该命令。

### 7. hexo list <type>
列出所有<type>类型的文件。type包括[post,page,draft,tag,category,route]。

### 8. hexo --debug
在终端中显示调试信息并记录到 debug.log。当碰到问题时，可以用调试模式重新执行一次，并提交调试信息到 GitHub。

在hexo中，source文件夹是放置几乎所有编辑文件的地方。在source文件夹中，如果有引入的第三方代码，例如：jquery，一定要加入到skip_render中。否则可能出现意想不到的错误。
