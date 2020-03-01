---
title: v-if和v-show
date: 2020-02-27 12:05:25
categories: css
tags: ['v-if','v-show']
---

## v-if和v-show的区别
1. v-if显示隐藏是将dom元素整个添加或删除，而v-show隐藏则是为该元素添加display:none，dom元素还在。
2. 性能消耗：v-if有更高的切换消耗；v-show有更高的初始渲染消耗。
3. 编译条件：v-if是惰性的，如果初始条件为假，则什么也不做；只有在条件第一次变为真时才开始局部编译（编译被缓存？编译被缓存后，然后再切换的时候进行局部卸载); v-show是在任何条件下（首次条件是否为真）都被编译，然后被缓存，而且DOM元素保留。

## display:none和visiblity:hidden的区别
1. display:none 不占页面空间，元素不占页面空间后，该元素和其内部元素的宽高值永远是0。visiblity:hidden占据原先页面空间。如果想隐藏又想取到宽高值，那就得用visiblity:hidden。
2. display:none 的子元素也一定无法显示，visiblity:hidden的子元素可以设置显示。display:none元素及其子元素都将隐藏，而visiblity:hidden元素的子元素却可以设置visibility: visible 显示出来。在这一点上，如果页面是比较复杂或者是不受控制的，就要慎重使用visiblity:hidden，因为保不齐哪个元素被设置成可见，影响显示效果。
3. display:none 引起页面重绘和回流， visiblity:hidden只引起页面重绘。visiblity:hidden看起来的性能比display:none好些在两者都能使用情况下，可先考虑visiblity:hidden。