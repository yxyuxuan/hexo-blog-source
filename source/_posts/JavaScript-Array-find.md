---
title: JavaScript 中Array数组的find()方法
date: 2017-11-19 19:20:47
tags: JavaScript
categories: JavaScript
---

### 1. Array.prototype.find()

`find()`是用于查找数组中值的方法，该方法返回数组中满足提供的测试函数的第一个元素的值。否则返回 `undefined` 。

```javascript
var findElement =  [12, 5, 22, 13, 52].find(element => element > 12); 
//findElement: 22
```
其中`findIndex()` 方法，它返回数组中找到的元素的索引，而不是其值。如果需要找到一个元素的位置或者一个元素是否存在于数组中，使用`Array.prototype.indexOf()` 或 `Array.prototype.includes()`。

### 2. 语法
```javascript
arr.find(callback[, thisArg])
```

#### 参数

##### callback

在数组每一项上执行的函数，接收 3 个参数：

##### element

当前遍历到的元素。

##### index

当前遍历到的索引。

##### array

数组本身。

##### thisArg 可选

可选，指定 `callback` 的 `this` 参数。

##### 返回值

当某个元素通过 `callback` 的测试时，返回数组中的一个值，否则返回 `undefined`。

### 3. 描述

`find` 方法对数组中的每一项元素执行一次 `callback` 函数，直至有一个 `callback` 返回 `true`。当找到了这样一个元素后，该方法会立即返回这个元素的值，否则返回 `undefined`。注意 `callback` 函数会为数组中的每个索引调用即从 0 到 lengh - 1，而不仅仅是那些被赋值的索引，这意味着对于稀疏数组来说，该方法的效率要低于那些只遍历有值的索引的方法。

`callback` 函数带有3个参数：当前元素的值、当前元素的索引，以及数组本身。

如果提供了 `thisArg` 参数，那么它将作为每次 `callback` 函数执行时的上下文对象，否则上下文对象为 `undefined`。

`find` 方法不会改变数组。

在第一次调用 `callback` 函数时会确定元素的索引范围，因此在 `find` 方法开始执行之后添加到数组的新元素将不会被`callback` 函数访问到。如果数组中一个尚未被`callback`函数访问到的元素的值被`callback`函数所改变，那么当`callback`函数访问到它时，它的值是将是根据它在数组中的索引所访问到的当前值。被删除的元素仍旧会被访问到。
