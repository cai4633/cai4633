---
title: 一篇文章搞懂promise
date: 2020-08-18 20:24:25
categories: js
tags: ["promise"]
---

## Promise 解决了什么问题？

在传统的异步编程中，如果异步之间存在依赖关系，就需要通过层层嵌套回调的方式满足这种依赖，如果嵌套层数过多，可读性和可以维护性都会变得很差，产生所谓的**“回调地狱”**。而 `Promise` 将嵌套调用改为**链式调用**，增加了可阅读性和可维护性。也就是说，Promise 用一种更加**友好的代码组织方式**解决了**异步嵌套**问题。Promise的缺陷是无法中断请求。
产生回调地狱的原因主要有两点：

1. (异步请求)嵌套调用，第一个函数的输出往往是第二个函数的输入；(Promise thenable 链式调用可以解决)
2. 处理多个异步请求并发，开发时需要同步请求最终的结果。（Promise.all()可以解决）

## then 的链式调用和值穿透

在我们使用 Promise 的时候，当 then 函数中 return 了一个值，不管是什么值，我们都能在下一个 then 中获取到，这就是所谓的 then 的链式调用。而且，当我们不在 then 中放入参数，例：promise.then().then()，那么其后面的 then 依旧可以得到之前 then 返回的值，这就是所谓的值的穿透。

## Promise 的 API

- Promise.resolve() 产生一个成功的 promise, 直接将值变成成功结果
- Promise.reject()  产生一个失败的 promise，直接将值变成错误结果
- Promise.all()   解决并发问题的，多个异步并发获取最终的结果（如果有一个失败则失败)
- Promise.race()  处理多个请求,谁先完成用谁的
- Promise.prototype.catch()   捕获 promise 的异常
- Promise.prototype.finally()  无论如何都会执行

## Promise/A+ 规范

1. promise 有三个状态：**pending，fulfilled，rejected**, 默认状态是 pending
2. new promise 时， 需要传递一个 executor(resolve, reject)执行器，**执行器立即执行**
3. promise 有一个**value 保存成功状态**的值，可以是 undefined/thenable/promise
4. promise 有一个**reason 保存失败状态**的值
5. promise **只能**从 pending 到 rejected, 或者从 pending 到 fulfilled，状态一旦确认，就不会再改变
6. promise 必须有一个 then 方法，then 接收两个参数，分别是 promise 成功的回调 onFulfilled, 和 promise 失败的回调 onRejected
7. 如果调用 then 时，promise 已经成功，则执行 onFulfilled，参数是 promise 的 value；
8. 如果调用 then 时，promise 已经失败，那么执行 onRejected, 参数是 promise 的 reason；
9. 如果 then 中抛出了异常，那么就会把这个异常作为参数，传递给下一个 then 的失败的回调 onRejected
10. then 的参数 onFulfilled 和 onRejected 可以缺省，如果 onFulfilled 或者 onRejected 不是函数，将其忽略，且依旧可以在下面的 then 中获取到之前返回的值；
11. promise 可以 then 多次，每次执行完 promise.then 方法后返回的都是一个**“新的 promise"**
12. 如果 then 的返回值是一个普通值，那么就会把这个结果作为参数，传递给下一个 then 的成功的回调中；
13. 如果 then 的**返回值是一个 promise，那么会等这个 promise 执行完**，promise 如果成功，就走下一个 then 的成功；如果失败，就走下一个 then 的失败；如果抛 1. 出异常，就走下一个 then 的失败
14. 如果 then 的返回值和 promise 是同一个引用对象，造成循环引用，则抛出异常，把异常传递给下一个 then 的失败的回调中
15. 如果 then 的返回值 x 是一个 promise，且 x 同时调用 resolve 函数和 reject 函数，则第一次调用优先，其他所有调用被忽略

`Promise实现代码：`

```
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';

const resolvePromise = (promise2, x, resolve, reject) => {
  // 自己等待自己完成是错误的实现，用一个类型错误，结束掉 promise  Promise/A+ 2.3.
  if (promise2 === x) {
    return reject(new TypeError('Chaining cycle detected for promise #<Promise>'))
  }
  // Promise/A+ 2.3.3.3.3 只能调用一次
  let called;
  // 后续的条件要严格判断 保证代码能和别的库一起使用
  if ((typeof x === 'object' && x != null) || typeof x === 'function') {
    try {
      // 为了判断 resolve 过的就不用再 reject 了（比如 reject 和 resolve 同时调用的时候）  Promise/A+ 2.3.3.1
      let then = x.then;
      if (typeof then === 'function') {
        // 不要写成 x.then，直接 then.call 就可以了 因为 x.then 会再次取值，Object.defineProperty  Promise/A+ 2.3.3.3
        then.call(x, y => { // 根据 promise 的状态决定是成功还是失败
          if (called) return;
          called = true;
          // 递归解析的过程（因为可能 promise 中还有 promise） Promise/A+ 2.3.3.3.1
          resolvePromise(promise2, y, resolve, reject);
        }, r => {
          // 只要失败就失败 Promise/A+ 2.3.3.3.2
          if (called) return;
          called = true;
          reject(r);
        });
      } else {
        // 如果 x.then 是个普通值就直接返回 resolve 作为结果  Promise/A+ 2.3.3.4
        resolve(x);
      }
    } catch (e) {
      // Promise/A+ 2.3.3.2
      if (called) return;
      called = true;
      reject(e)
    }
  } else {
    // 如果 x 是个普通值就直接返回 resolve 作为结果  Promise/A+ 2.3.4
    resolve(x)
  }
}

class Promise {
  constructor(executor) {
    this.status = PENDING;
    this.value = undefined;
    this.reason = undefined;
    this.onResolvedCallbacks = [];
    this.onRejectedCallbacks= [];

    let resolve = (value) => {
      if(this.status ===  PENDING) {
        this.status = FULFILLED;
        this.value = value;
        this.onResolvedCallbacks.forEach(fn=>fn());
      }
    }

    let reject = (reason) => {
      if(this.status ===  PENDING) {
        this.status = REJECTED;
        this.reason = reason;
        this.onRejectedCallbacks.forEach(fn=>fn());
      }
    }

    try {
      executor(resolve,reject)
    } catch (error) {
      reject(error)
    }
  }

<!-- 静态方法 -->
  static resolve(data){
    return new Promise((resolve,reject)=>{
      resolve(data);
    })
  }

  static reject(reason){
    return new Promise((resolve,reject)=>{
      reject(reason);
    })
  }

  static race(promises) {
    return new Promise((resolve, reject) => {
      // 一起执行就是for循环
      for (let i = 0; i < promises.length; i++) {
        let val = promises[i];
        if (val && typeof val.then === 'function') {
          val.then(resolve, reject);
        } else { // 普通值
          resolve(val)
        }
      }
    });
  }

  static all(values) {
    if (!Array.isArray(values)) {
      const type = typeof values;
      return new TypeError(`TypeError: ${type} ${values} is not iterable`)
    }
    return new Promise((resolve, reject) => {
      let resultArr = [];
      let orderIndex = 0;
      const processResultByKey = (value, index) => {
        resultArr[index] = value;
        if (++orderIndex === values.length) {
            resolve(resultArr)
        }
      }
      for (let i = 0; i < values.length; i++) {
        let value = values[i];
        if (value && typeof value.then === 'function') {
          value.then((value) => {
            processResultByKey(value, i);
          }, reject);
        } else {
          processResultByKey(value, i);
        }
      }
    });
  }

  then(onFulfilled, onRejected) {
    //解决 onFufilled，onRejected 没有传值的问题
    //Promise/A+ 2.2.1 / Promise/A+ 2.2.5 / Promise/A+ 2.2.7.3 / Promise/A+ 2.2.7.4
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v => v;
    //因为错误的值要让后面访问到，所以这里也要跑出个错误，不然会在之后 then 的 resolve 中捕获
    onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err };
    // 每次调用 then 都返回一个新的 promise  Promise/A+ 2.2.7
    let promise2 = new Promise((resolve, reject) => {
      if (this.status === FULFILLED) {
        //Promise/A+ 2.2.2
        //Promise/A+ 2.2.4 --- setTimeout
        setTimeout(() => {
          try {
            //Promise/A+ 2.2.7.1
            let x = onFulfilled(this.value);
            // x可能是一个proimise
            resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            //Promise/A+ 2.2.7.2
            reject(e)
          }
        }, 0);
      }

      if (this.status === REJECTED) {
        //Promise/A+ 2.2.3
        setTimeout(() => {
          try {
            let x = onRejected(this.reason);
            resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            reject(e)
          }
        }, 0);
      }

      if (this.status === PENDING) {
        this.onResolvedCallbacks.push(() => {
          setTimeout(() => {
            try {
              let x = onFulfilled(this.value);
              resolvePromise(promise2, x, resolve, reject);
            } catch (e) {
              reject(e)
            }
          }, 0);
        });

        this.onRejectedCallbacks.push(()=> {
          setTimeout(() => {
            try {
              let x = onRejected(this.reason);
              resolvePromise(promise2, x, resolve, reject)
            } catch (e) {
              reject(e)
            }
          }, 0);
        });
      }
    });

    return promise2;
  }
}

Promise.prototype.catch = function(errCallback){
  return this.then(null,errCallback)
}

Promise.prototype.finally = function(callback) {
  return this.then((value)=>{
    return Promise.resolve(callback()).then(()=>value)
  },(reason)=>{
    return Promise.resolve(callback()).then(()=>{throw reason})
  })  
}
```

## 参考
[你能手写一个 Promise 吗](https://zhuanlan.zhihu.com/p/183801144)