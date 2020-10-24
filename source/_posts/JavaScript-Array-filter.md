---
title: JavaScript 中Array数组的filter()方法
date: 2017-11-12 20:13:50
tags: JavaScript
categories: JavaScript
---

### 1. Array.prototype.filter()

`filter()` 是用于筛选的方法，该方法用于创建一个符合条件的新数组，新数组中包含通过函数测试的所有元素。

```javascript
var filteredArr = [1, 22, 6, 11, 34, 2, 9].filter(element => element > 9);
//filteredArr: [22, 11, 34]
```





### 2. 语法

```javascript
var new_array = arr.filter(callback[, thisArg]);
```

##### callback

用来测试数组的每个元素的函数。调用时使用参数 (element, index, array)。返回true表示保留该元素（通过测试），false则不保留。

##### thisArg

可选。执行 callback 时的用于 this 的值。

##### 返回值

一个新的通过测试的元素的集合的数组

### 3. 描述

filter 为数组中的每个元素调用一次 callback 函数，并利用所有使得 callback 返回 true 或 等价于 true 的值 的元素创建一个新数组。callback 只会在已经赋值的索引上被调用，对于那些已经被删除或者从未被赋值的索引不会被调用。那些没有通过 callback 测试的元素会被跳过，不会被包含在新数组中。

callback 被调用时传入三个参数：

- 元素的值
- 元素的索引
- 被遍历的数组

如果为`filter` 提供一个 thisArg 参数，则它会被作为 callback 被调用时的 this 值。否则，callback 的 this 值在非严格模式下将是全局对象，严格模式下为 undefined。
The thisvalue ultimately observable by callback is determined according to the usual rules for determining thethis seen by a function.

`filter` 不会改变原数组。

`filter` 遍历的元素范围在第一次调用 `callback` 之前就已经确定了。在调用 `filter` 之后被添加到数组中的元素不会被 `filter` 遍历到。如果已经存在的元素被改变了，则他们传入 `callback` 的值是 `filter` 遍历到它们那一刻的值。被删除或从来未被赋值的元素不会被遍历到。
