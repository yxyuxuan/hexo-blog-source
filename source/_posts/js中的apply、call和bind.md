---
title: js中的apply、call和bind
date: 2020-01-15 13:59:45
tags: Javascript
categories: JavaScript
---

> apply()、call()和bind()方法都是Function.prototype对象中的方法，而所有的函数都是Function的实例。这三个方法的用法非常相似，将函数绑定到上下文中，即用来改变函数中this的指向。

### 示例：

```javascript
  function Person(name){
    this.name = name;
  }
  
  let obj = {name: 'lucy'};// 注意这是一个普通对象，它不是Person的实例
  
  Person.prototype = {
    constructor: Person,
    showName: function(){
      console.log(this.name);
    }
  }
  
  let person = new Person('lily');
  person.showName(); // lily
  
  //使用call
  person.showName.apply(obj,[param1, param2, param3]); // lucy
  
  //使用apply
  person.showName.call(obj,param1, param2, param3); // lucy
  
  //使用bind
  let bind = person.showName.bind(obj); // 返回一个函数
  bind(); // lucy
```

apply和call都是直接执行函数调用。

bind是绑定，执行需要再次调用。

apply和call的区别是从第二个参数开始，apply接受数组作为参数，而call是接受逗号分隔的无限多个参数列表。

### 应用：

1. ##### 将伪数组转化为数组（含有length属性的对象，函数的参数的类数组对象arguments）

   ```JavaScript
   //含有length属性的对象
   let obj = {
   	0: "1ily",
   	1: "women",
   	2: 20,
   	length: 3 // 一定要有length属性
   };
   console.log(Array.prototype.slice.call(obj)); // ["1ily", "women", 20]
   
   //函数的参数的类数组对象arguments
   function fun() {
       return Array.prototype.slice.call(arguments);
   }
   console.log(fun(1,2,3,4,5)); // [1, 2, 3, 4, 5]
   ```

   

2. **利用call和apply做继承**

   ```JavaScript
   function Animal(name){      
       this.name = name;      
       this.showName = function(){      
           console.log(this.name);      
       }      
   }      
   
   function Cat(name){    
       Animal.call(this, name);    
   }      
   
   // Animal.call(this) 的意思就是使用this对象代替Animal对象，那么
   // Cat中不就有Animal的所有属性和方法了吗，Cat对象就能够直接调用Animal的方法以及属性了
   var cat = new Cat("1ily");     
   cat.showName();   //1ily
   ```

3. **求数组中的最大值和最小值**

   ```JavaScript
   let arr = [1,2,3,89,46]
   let max = Math.max.apply(null,arr)//89
   let min = Math.min.apply(null,arr)//1
   ```

4. **数组的追加**

   ```JavaScript
   let arr1 = [1,2,3]
   let arr2 = [8,9,10]
   let total = [].push.apply(arr1, arr2) //6
   // arr1 [1, 2, 3, 8,9,10]
   // arr2 [8,9,10]
   ```

5.  **总结**

   三者都可以改变函数的this对象指向。

   三者第一个参数都是this要指向的对象，如果如果没有这个参数，默认指向全局window。

   三者都可以传参，但是apply是数组，而call是有顺序的传入。

   bind 是返回对应函数，便于稍后调用。apply 、call 则是立即执行 。

