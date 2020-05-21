---
title: npm常用的命令
date: 2019-10-30 22:06:14
categories: 前端
tags: npm
---
最近常用到一些npm的命令，今天总结一下，方便以后查阅。
使用npm，需要提前安装**node.js**。
### 查询npm和node.js版本
```
npm -v
node -v
```

### 列出npm所有的命令用法
```
npm -l
npm <command> -h    //某条指令的详细说明
```

### 安装npm包
```
npm i <package name> [ -g,-S,--save-dev]
npm install -g cnpm --registry=https://registry.npm.taobao.org   //安装国内淘宝镜像
```

### 卸载npm包
```
npm un <package name> [ -g,-S,--save-dev]
```

### 升级npm包
```
npm up <package name> [ -g,-S,--save-dev]
```

### 查看本地已安装的包
```
npm ls [-g]
```

### 查看某个包在npm服务器上的版本信息
```
npm info <package name>     //查看包在npm服务器上最新版本的详细信息 
npm view <package name> versions    //查看包在npm服务器上的所有版本号（仅显示版本号） 
npm view <package name> version    //查看包在npm服务器上的最新版本号（仅显示版本号） 
```

### 清空npm本地缓存
```
npm cache clear      //当使用相同版本号发布新版本代码时，更新前需执行此命令
```
### 设置淘宝镜像
`npm config set registry https://registry.npm.taobao.org`

### 设置官方源
`npm config set registry https://www.npmjs.org `
