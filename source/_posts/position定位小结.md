---
title: position定位小结
date: 2020-02-23 04:26:44
categories: css
tags: ['position', 'z-index', 'absolute' ,'BFC','层叠上下文']
---

## position属性
position属性的值有static(默认值)，relative，absolute，fixed。

### static:
所有元素默认值。即没有定位，元素仍出现在正常文本流中。
特性：
1. ***忽略 top, bottom, left, right或者z-index声明***。

### relative:
相对定位元素会**相对于它自己在正常流中的默认位置**偏移。
特性：
1. 相对于自己偏移，但原来所占的位置将继续占有，没有脱离标准文档流，**可设置z-index属性**。
2. 每次移动是以自己的左上角为基点移动。

### absolute: 
生成绝对定位的元素。**相对于static定位以外**的第一个父元素进行定位。脱离正常文档流，**可设置z-index属性**实现层叠。
特性：
1. 绝对定位偏移基准点是父元素的**padding-box左上角**，即absolute元素的margin-box相对于父元素的padding-box偏移。
2. 没有设置定位值(left,right,top,bottom)的元素，仍处在原本在标准流中的位置。但是它已经脱离了文本流，不会影响兄弟元素的布局。
3. 绝对定位元素的宽高百分比是相对于其最近的父级别定位元素的padding-box的大小来计算的。
4. 定位值（left,right,top,bottom）的百分比也是相对于父元素的padding-box宽高来计算的。
5. 当left，right，top，bottom无法同时满足时，优先满足left和top。

### fixed:    
固定定位。一般**相对于浏览器窗口**进行定位，脱离正常文档流，可以设置z-index。
特性：
1. 固定定位相邻父元素出现transform和perspective属性，那么此时固定元素相对于父元素定位而不是浏览器窗口。
2. fixed固定定位对于IE6,7,8不兼容。
3. fixed固定定位元素的宽高百分比是相对于浏览器窗口（body的border-box）来计算的。
4. 固定定位偏移基准点是视口的左上角，即fixed元素的margin-box相对于body的border-box偏移。

任何元素都可以定位。不过***绝对或固定定位会生成一个BFC***。

## 小结：
1. 触发 BFC
BFC可看成XY方向上标准流布局图层，只要元素满足下面任一条件即可触发 BFC 特性：
    1. body 根元素
    2. 浮动元素：float 除 none 以外的值
    3. 绝对定位元素：position: (absolute、fixed)
    4. display 为 inline-block、table-cells、flex
    5. overflow 除了 visible 以外的值 (hidden、auto、scroll)

2. z-index（默认值auto）
    1. 只有position为relative/absolute/fixed时（除了static）,Z-index才生效。
    2. z-index:0和z-index:auto层叠水平一样。但是z-index:auto不会产生层叠上下文，而z-index:0(或者数值)都会产生新的层叠上下文。
    3. 同一层叠上下文，z-index越大层叠水平越高，层叠水平相同后来居上。
    4. 不同层叠上下文比较，要比较其父元素层叠水平。

3. 层叠顺序和层叠上下文
    1. 七层层叠顺序
    ![七层层叠顺序](/images/123.png)
    2. 层叠上下文
    层叠上下文可看成Z方向上布局图层。元素满足下面任一条件即可产生新的层叠上下文：
        1. html根元素 。
        1. position值为 absolute|relative，且 z-index值不为 auto。
        1. position 值为 fixed|sticky。
        1. z-index 值不为 auto 的flex元素，即：父元素 display:flex|inline-flex。
        1. opacity 属性值小于 1 的元素。
        1. transform 属性值不为 none的元素。
        1. mix-blend-mode 属性值不为 normal 的元素。
        1. filter、 perspective、 clip-path、 mask、 mask-image、 mask-border、 motion-path值不为none的元素 。
        1. perspective 值不为 none 的元素。
        1. isolation 属性被设置为 isolate 的元素 。
        1. will-change 中指定了任意 CSS 属性，即便你没有直接指定这些属性的值。
        1. -webkit-overflow-scrolling 属性被设置 touch的元素。
。
