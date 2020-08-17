---
title: ES5和ES6中的this指向
date: 2020-08-10 21:19:14
tags: Javascript
categories: 前端
---

> 相信每个前端同学对于this指向都有过探索，虽然平时的开发中极少使用this，但是踩过坑以后都会意识到this指向的重要性，笔试的时候出现率是极高的......

### ES5中的this

**一般的，this指向函数运行（调用）时所在的执行环境**

- 在普通函数中的this总是代表他的直接调用者，默认情况下指向windos
- 在严格模式下，没有直接调用者的函数中的this是undefined使用  
- call,apply,bind，this指向的是绑定的对象
- 在全局的调用函数里this指向window
- 对象函数调用，那个函数调用this就指向那个对象
- 定时器（setTimeout，setInterval）中的this指向全局变量对象
- 构造函数中的this指向构造出来的新对象

例1：

```javascript
var obj = {
    b:"10",
    n:"11",
    a: function() {
        console.log(this);
        console.log(this.a);
        console.log(this.n);
        console.log(this.b);
    }
}

var b = obj.a;
b();
// window
// undefined
// undefined
// f() {
       console.log(this);
       console.log(this.a);
       console.log(this.n);
       console.log(this.b);
   }
```

分析代码：

```javascript
var b = obj.a;    
var b = function() {
    console.log(this);
    console.log(this.a);
    console.log(this.n);
    console.log(this.b);
}
```

b()函数调用时，其所在的执行环境是全局环境，所以this指向全局变量对象window

例2：

```
function fun() {
    console.log("id1:", this.id);
    console.log("this1:", this);
    setTimeout(function() {
        console.log("id2:", this.id);
        console.log("this2:", this);
    }, 0);
}

var id = 10;

fun();
// id1: 10
// this1: window
// id2: 10
// this2: window


fun.call({id:20});
// id1: 20
// this1: {id:20}
// id2: 10
// this2: window
```

使用fun函数的call方法改变了fun函数调用时函数体内this的指向（{id: 42}这个对象，但setTimeout回调函数中的this依旧指向window对象（因为setTimeout在全局环境中运行）。

### ES6中的this

- 箭头函数没有自己的this，它的this是继承来的，默认指向在定义它时所在的对象。即箭头函数中的this是指向外层代码块的this。

例：

```javascript
function fun() {
    console.log("id1:", this.id);
    console.log("this1:", this);
    setTimeout(() => {
        console.log("id2:", this.id);
        console.log("this2:", this);
    }, 0);
}

var id = 10;

fun();

// Chrome
// id1: 10
// this1: window
// id2: 10
// this2: window

fun.call({id: 20});

// Chrome
// id1: 20
// this1: {id: 20}
// id2: 20
// this2: {id: 20}
```

因为箭头函数（setTimeout回调）没有自己的this，导致其内部的this引用了外层代码块的this，即fun函数的this。

因为箭头函数内部的this是指向外层代码块的this（最近的this foo函数）的，所以我们可以通过改变外层代码块的this的指向从而改变箭头函数中this的指向（使用fun函数的call方法）。

例子：

```jsx
var id = 10;
let fun = () => {
    console.log(this.id)
};
fun();     // 10
fun.call({ id: 20 });     // 10
fun.apply({ id: 20 });    // 10
fun.bind({ id: 20 })();   // 10
```

解释：箭头函数中的this指向外层代码块的this，该代码块的this是window，代码中fun是箭头函数，无法改变this指向，因为箭头函数本身没有this，所以打印的都是10。