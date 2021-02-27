---
title: React中的Component和PureComponent区别
date: 2020-09-04 21:00:13
tags: React
categories: React
---

> 用React的Component或PureComponent来创建类组件时都会自动调用`shouldComponentUpdate()`生命周期函数，但区别是什么？



**区别：**

使用Component组件时，当 props 或 state 发生变化时，`shouldComponentUpdate()` 会在渲染执行之前被调用，返回值默认为 true。不会进行比较来确定返回值是true或false，默认就返回true。

使用PureComponent组件时，当props或state发生变化时，PureComponent会对props和state进行浅比较来决定`shouldComponentUpdate()` 函数的返回值是true或false。



**pureComponent的优缺点**

优点： 不需要手写`shouldComponentUpdate()`，通过浅比较来提升性能，可以减少不必要的render。当组件更新时，PureComponent的`shouldComponentUpdate()`函数会对props和state做了一个浅对比，如果组件的state和prop都没有发生变化，就不会触发render方法，省去了virtual DOM的diff和重新生成的过程，从而提升了性能。

缺点：由于进行的是浅比较，可能由于深层的数据不一致导致而产生错误的否定判断，从而导致页 面得不到更新。不适合使用在含有多层嵌套对象的state和prop中。

