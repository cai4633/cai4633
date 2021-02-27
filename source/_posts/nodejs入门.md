---
title: nodejs 入门
date: 2020-12-31 14:44:52
categories: nodejs
tags: nodejs
---

## 什么是 nodejs?

对于新手而言，nodejs 可能会**被误认为是后端的 js**。我们要知道，nodejs **不是一个 web 框架**，也**不是一门编程语言**，它是一个让 js 也可以调用系统接口，开发后端应用的**运行环境**。

## nodejs 技术架构

我们知道了 nodejs 是一个平台。那么它底层的技术架构是怎么实现的呢？
![nodejs 工作流程](/images/nodejs_process.jpg)
nodejs 给用户提供了常用的 **API** 。这些 api 我们可以直接在 js 中使用。js 是运行在**V8 引擎**上的，V8 引擎通过 **nodejs Bindings** 与 **libUV** 通信。除此之外，我们还可以**自定义 c/c++ 插件**拓展功能。

### nodejs Bindings

我们都知道 c/c++执行效率非常高，那么我们想用 js 直接调用 c/c++库 怎么办？nodejs Bindings 就是连接 js 和 c/c++ 的中间桥梁。Binding 将 c/c++ 编译为 .node 文件，这个文件可以直接被 js 获取并执行。

### V8 引擎

V8 引擎负责将 js 源代码变成本地代码并执行。它**维护了调用栈**，确保 js 的执行顺序。负责**内存管理和垃圾回收**。实现了 js 的标准库，但不提供 DOM API, DOM API 是浏览器提供的。
V8 引擎本身是多线程的，但是执行 js 是单线程，它自身有 EventLoop 机制，但是 **nodejs 的 EventLoop 是基于 libuv 的**。

### libUV

libUV 是 nodejs 之父 Ryan 开发的一个**高性能跨平台 异步 I/O 库**，底层由 c 语言编写，有自己的一套 eventLoop 机制。它专为 Node.js 而设计，后因**事件驱动**的异步 IO 的高效逐渐被其他语言和项目采纳。libUV 通常可用于 TCP / UDP / DNS / FileSystem 等异步操作。
