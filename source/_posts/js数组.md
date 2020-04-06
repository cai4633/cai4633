---
title: js数组
date: 2020-04-05 12:03:32
categories: js
tags: ['js','Array']
---
js 数组就是有序数据的集合。数组的本质是**原型链中有Array.prototype的对象**。Array对象是用于构造数组的全局对象。
## 申明数组
1. `let arr = [1,2,3]`
2. `let arr = new Array(3)   //new可以省略，声明一个length为3的空数组，arr[1] === undefined`
3. `let arr = new Array(3,3)   //new可以省略，声明一个数组[3,3]`

## 伪数组
拥有数字key和length属性，但原型链中*没有Array.prototype*的对象称为伪数组。常见的伪数组有：arguments对象、DOM操作中获得的Elements集合。

## 数组常用的API
1. Array.prototype.forEach()
` arr.forEach(callback)  //数组的每个元素执行一次callback函数。` 

2. Array.prototype.sort()
```
//升序排列
let numbers = [4, 2, 5, 1, 3]; 
numbers.sort((a, b) => a - b);          // 在原数组上排序，改变原数组
```

3. Array.prototype.join()
` let string = arr.join(',')  //将数组的所有元素连接成一个字符串并返回这个字符串。如果数组只有一个项目，那么将返回该项目而不使用分隔符 `

4. Array.prototype.concat()
` let newArr = arr1.concat(arr2,arr3)  //合并两个或多个数组。不更改现有数组，而是返回一个新数组。 ` 

5. Array.prototype.toString()
`toString()返回一个字符串，表示指定的数组及其元素。`

6. Array.prototype.map()
返回一个新数组，其结果是原数组中的每个元素都调用callback函数后返回的结果。
```
const array1 = [1, 4, 9, 16];
// pass a function to map
const newMap = array1.map(x => x * 2);
```

7. Array.prototype.filter()
`let newArray = array.filter(word => word.length > 6);    //返回一个新数组, 其包含通过所提供函数实现的测试的所有元素` 

8. Array.prototype.reduce()
对数组中的每个元素执行一次reducer函数(升序执行)，并返回最终的ACC结果。
```
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;
console.log(array1.reduce(reducer));     // 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));  // 5 + 1 + 2 + 3 + 4
``` 

9. Array.from(arrayLike)   
    传入伪数组，返回一个数组
10. Array.isArray(object)        
    传入一个对象，判断是否是数组，返回值boolean
