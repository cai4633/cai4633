---
title: Promise的理解
date: 2019-10-25 21:41:21
categories:
tags:
---
## Promise
Promise对象是解决异步问题的一种方法。它的基本用法如下：
{% codeblock lang:js %}
    const promise = new Promise((resolve,reject) => {
        ...(async code)         //一段异步代码     pending状态
        if(succeed){            //fulfilled状态
            resolve();
        }else{                  //rejected 状态
            reject();
        }
    })
    promise.then(resolve,reject).then(...)
{% endcodeblock %}
它有以下特点：
1. new promise参数中的函数会立即执行，等同于同步代码。
2. promise对象的状态不受外界影响，而且状态一旦改变就不会再变。它有三种状态：pending（进行中），fulfilled（已成功），rejected（已失败）。在new promise中执行resolve()时，状态由pennding变成fulfilled，执行reject()时，状态会由pending变成rejected.
3. 当promise状态改变就会触发then()中的回调函数，这个回调函数属于microTask。优先级高于macroTask但低于同步代码。

<!-- ## Promise常用api -->