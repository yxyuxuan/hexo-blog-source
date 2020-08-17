---
title: JS数据类型的判断
date: 2020-02-13 21:44:37
tags: Javascript
categories: 前端
---

### typeof

- 可以判断数据类型，它返回表示数据类型的字符串（返回结果只能包括number,boolean,string,function,object,undefined）；
- 可以使用typeof判断变量是否存在（如if(typeof a!="undefined"){...}）；
- Typeof 运算符的问题是无论引用的对象是什么类型    它都返回object

```csharp
typeof {} // object
typeof  [1,2] // object
typeof /\s/ //object
```

### instanceof

原理 因为A instanceof B 可以判断A是不是B的实例，返回一个布尔值，由构造类型判断出数据类型

```jsx
console.log(arr instanceof Array ); // true
console.log(date instanceof Date ); // true
console.log(fn instanceof Function ); // true
```

### Object下的toString.call()方法

```jsx
Object.prototype.toString.call();

console.log(toString.call(123)); //[object Number]
console.log(toString.call('123')); //[object String]
console.log(toString.call(undefined)); //[object Undefined]
console.log(toString.call(true)); //[object Boolean]
console.log(toString.call({})); //[object Object]
console.log(toString.call([])); //[object Array]
console.log(toString.call(function(){})); //[object Function]
```

### 根据对象的contructor判断：

```tsx
console.log('数据类型判断' -  constructor);
console.log(arr.constructor === Array); //true
console.log(date.constructor === Date); //true
console.log(fn.constructor === Function); //true
```

