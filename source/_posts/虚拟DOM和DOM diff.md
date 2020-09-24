---
title: 虚拟 DOM 和 DOM diff
date: 2020-09-18 00:02:24
categories: 前端
tags: ["vDOM", "DOM diff"]
---

## 虚拟 DOM

### 虚拟 DOM 是什么

虚拟 dom 的本质就是一个能代表 dom 树的**JS 对象。** 它是相对于浏览器所渲染出来的真实 dom 而言的。它包括标签类型、子元素、属性以及事件等。

在虚拟 DOM 技术出现之前，我们要改变页面内容只能通过遍历查询 dom 树的方式找到需要修改的 dom 然后修改 dom 属性或者结构等，来达到更新 ui 的目的。因为每次查询 dom 几乎都需要遍历整颗 dom 树。所以这种方式比较消耗性能。虚拟 dom 对象（ js 对象）以对象嵌套的方式来表示 dom 树，查找 js 对象的属性变化要比查询 dom 树的性能开销小。

### 虚拟 DOM 实例

虚拟 dom 在不同的框架中，有着不一样的具体形式。
**React 虚拟 DOM** :

```
const vNode = {
  key: null,
  type:'div',
  ref: null,
  props: {
    children:[
      {type:'span' ...},
      {type:'span' ...},
    ],
    onClick:()=>{},
    className: 'main'
  }
  ...
}
```

**Vue 虚拟 DOM** :

```
const vNode = {
  tag:'div',
  data: {
    on:{
      click:()=>{}
    },
    class: 'main'
  },
  children:[
    {tag:'span' ...},
    {tag:'span' ...},
  ],
  ...
}
```

### 创建虚拟 DOM
React 内置函数：`React.createElement(type,[props],[...children])`
简化写法： jsx + babel
Vue 内置函数：`h(tag,data,children)`
简化写法： vue template + vue-loader

### 虚拟 DOM 的优点

1. 减少 dom 操作次数
   真实 dom 插入 1000 个节点需要进行 1000 次 dom 操作，虚拟 dom 可以将多次操作合并为一次 dom 操作。
2. 减少 dom 操作范围
   通过 DOM diff 算法对比新旧虚拟 dom 树,去掉不必要的 dom 操作。例如：添加 1000 个节点，虚拟 dom 通过对比发现有 900 个相同的节点已经存在于相同的层级位置，那么虚拟 dom 只会添加 100 个节点。
3. 跨平台
   由于虚拟 dom 本质是 JS 对象，所以它可以变成 DOM、小程序、IOS 应用或者安卓应用。

### 虚拟 DOM 的缺点
1. 需要额外的创建函数
2. 需要额外的转义构建工具
3. 节点数量少时虚拟 dom 效率高，但是节点数量多时虚拟 dom 性能比不上原生dom（vue接近原生dom，react性能偏差）。

## DOM diff 
### DOM diff 是什么
它的本质是一个函数，类似于`patches = patch(newVDom, oldVDom)`。其中patches就是需要进行的 dom 操作。diff 算法在执行时有三个维度，分别是**tree diff、component diff 和 element diff，**执行时按层级顺序依次执行，它们的差异仅仅因为 diff 粒度不同、执行先后顺序不同。

### DOM diff 的优点
DOM diff算法会对比 oldNode 与 newNode 的区别，从而**减少不必要的渲染**。

### DOM diff 的问题
DOM diff在同层级对比中有bug。造成页面渲染错误。**同一层级的一组节点**可以通过唯一的id进行区分, 所以可以给节点设定唯一的key。从而消除bug。**key只能是number和string类型，一定不要用index作为key值。**

### 几个使用key的场景
1. v-for (vue)
2. 同一层级有相同标签节点的**transition动画**

