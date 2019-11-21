---
title: JS中的Event-Loop（事件循环）
date: 2019-11-21 14:21:03
tags: JavaScript
categories: 前端
---

> Javascript是一门单线程的非堵塞的脚本语言，也是用来与浏览器交互的语言。
>
> 单线程是：代码执行的任何时候，都只有一个主线程来处理所有的任务。
>
> 非堵塞：当代码需要进行一项异步任务的时候，主线程会挂起这个异步任务，继续向下执行，然后在异步任务返回结果的时候再根据一定规则去执行相应的回调。 

## JS中的堆、栈、队列

#### 堆（heap）

堆（heap）是指程序运行时申请的动态内存，在JS运行时用来存放对象。

#### 栈（stack）

栈（stack）遵循的原则是“先进后出”，Javascript中的基本数据类型与指向对象的地址的指针存放在栈内存中，此外还有一块栈内存用来执行JS主线程--执行栈（execution context stack）。

#### 队列（queue）

队列（queue）遵循的原则是“先进先出”，JS中除了主线程之外还存在一个“任务队列”，任务队列又分为微任务和宏任务。

## 什么是Event Loop线程？

**在程序中设置两个线程：一个负责程序本身的运行，称为"主线程"；另一个负责主线程与其它进程（主要是各种I/O操作）的通信，被称为"Event Loop线程"** 

Event Loop线程处理的任务被分为两类即 微任务（micro task）和宏任务（macro task） ，这两个任务分别维护一个队列，都是采用先进先出的策略进行执行。

宏任务：setTimeout、setInterval、I/O。

微任务：Promise.then、 MutationObserver。

当遇到异步请求的时候，主线程就让Event Loop线程去通知相应的异步请求程序，然后接着往后运行，等到异步程序完成操作，Event Loop线程再把结果返回主线程，主线程就调用事先设定的回调函数，完成整个任务。 

当前栈执行完后，Event Loop线程首先会执行微任务队列中事件，然后执行宏任务队列的事件，如果这时发现了微任务，则执行微任务，然后执行宏任务，反复循环至所有事件执行完毕。微任务永远在宏任务之前执行。

**Example：**

```js
console.log(1);
setTimeout(function() {
    console.log(2);
}, 0);
setTimeout(function() {
    new Promise(function(resolve) {
        console.log(6);
        resolve(Date.now());
    }).then(function() {
        console.log(7);
    });
    setTimeout(function() {
        console.log(10);
	}, 0);
}, 0);
new Promise(function(resolve) {
    console.log(8);
    resolve(Date.now());
}).then(function() {
    console.log(9);
});
new Promise(function(resolve) {
    console.log(3);
    resolve(Date.now());
}).then(function() {
    console.log(4);
});
console.log(5);
```

解析执行步骤：

1. 主线程先执行log(1)
2. log(2)加入宏任务，log(6,7,10)加入宏任务，执行log(8)，把log(9)加入微任务
3. 执行log(3)，把log(4)加入微任务
4. 执行log(5)
5. 发现微任务，开始处理微任务，先进先出的规则，执行log(9)，log(4)，微任务执行完毕。
6. 接着处理宏任务，发现宏任务，执行log(2)，执行log(6)，把log(7)加入微任务，把log(10)加入宏任务
7. 发现微任务，执行微任务，执行log(7)，微任务执行完毕。
8. 接着执行宏任务，执行log(10)，宏任务和微任务均执行完毕。

执行结果： 1，8，3，5，9，4，2，6，7，10