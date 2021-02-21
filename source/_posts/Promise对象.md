---
title: Promise对象
date: 2019-10-15 15:43:42
tags: JavaScript
categories: JavaScript
---

>  Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大 。 Promise出现之前解决异步编程用回调，当出现按顺序请求多个异步代码时就只能用回调嵌套来解决，当请求的异步代码很多时就会出现 ”回调地狱“。Promise的出现使得回调看起来变得可控。使用async，await使得异步代码看起来是在同步运行。

ES6 规定，**`Promise`对象**是一个构造函数，用来生成`Promise`实例。 

```
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

 `Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。 它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

`resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；`reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

`Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。

```javascript
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

`then`方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`resolved`时调用，第二个回调函数是`Promise`对象的状态变为`rejected`时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受`Promise`对象传出的值作为参数。

**有了 Promise 对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数**。此外，Promise 对象提供统一的接口，使得控制异步操作更加容易。

**Promise规范如下：**

- 一个promise可能有三种状态：**等待（pending）、已完成（fulfilled）、已拒绝（rejected）**

- 一个promise的状态只可能从“等待”转到“完成”态或者“拒绝”态，不能逆向转换，同时“完成”态和“拒绝”态不能相互转换

- promise必须实现`then`方法（可以说，then就是promise的核心），而且then必须返回一个promise，同一个promise的then可以调用多次，并且回调的执行顺序跟它们被定义时的顺序一致

- then方法接受两个参数，第一个参数是成功时的回调，在promise由“等待”态转换到“完成”态时调用，另一个是失败时的回调，在promise由“等待”态转换到“拒绝”态时调用。同时，then可以接受另一个promise传入，也接受一个“类then”的对象或方法，即thenable对象。


**promise 的缺点：**

- 无法取消 Promise，一旦新建它就会立即执行，无法中途取消。
- 如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。第三，当处于 Pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

**用代码实现Promise：**

```javascript
function myPromise(constructor){ let self=this;
  self.status="pending" //定义状态改变前的初始状态 
  self.value=undefined;//定义状态为resolved的时候的状态 
  self.reason=undefined;//定义状态为rejected的时候的状态 
  function resolve(value){
    //两个==="pending"，保证了了状态的改变是不不可逆的 
    if(self.status==="pending"){
      self.value=value;
      self.status="resolved"; 
    }
  }
  function reject(reason){
     //两个==="pending"，保证了了状态的改变是不不可逆的
     if(self.status==="pending"){
        self.reason=reason;
        self.status="rejected"; 
      }
  }
  //捕获构造异常 
  try{
      constructor(resolve,reject);
  }catch(e){
    reject(e);
    } 
}
myPromise.prototype.then=function(onFullfilled,onRejected){ 
  let self=this;
  switch(self.status){
    case "resolved": onFullfilled(self.value); break;
    case "rejected": onRejected(self.reason); break;
    default: 
  }
}

// 测试
var p=new myPromise(function(resolve,reject){resolve(1)}); 
p.then(function(x){console.log(x)})
//输出1
```

