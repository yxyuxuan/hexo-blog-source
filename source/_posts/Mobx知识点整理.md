---
title: Mobx知识点整理
date: 2018-04-21 13:37:04
tags: Mobx
categories: React
---

> 最近老大让我用Mobx编写供应商系统，写着代码看着文档很心累，现在写完了第一版本，mobx也算用的顺手了，今天整理了mobx的一些基础知识点，以供需要时瞄瞄！！！



**MobX** 是一个库，使得状态管理变得简单和可扩展。

MobX 支持单向数据流，也就是**动作**改变**状态**，而状态的改变会更新所有受影响的**视图**。![Action, State, View](http://cn.mobx.js.org/images/action-state-view.png)



当**状态**改变时，所有**衍生**都会进行**原子级的自动**更新。因此永远不可能观察到中间值。



<u>一般需要掌握以下四点就可以熟练的使用Mobx</u>

1.  @observer

   函数/装饰器可以用来将 React 组件转变成响应式组件。

   它用 `mobx.autorun` 包装了组件的 render 函数以确保任何组件渲染中使用的数据变化时都可以强制刷新组件。 `observer` 是由单独的 `mobx-react` 包提供的。

   

2. @observable 

   装饰器使状态变成可观察的

   `@observable classProperty = value`

   

3.   @computed

   计算值(computed values)是可以根据现有的状态或其它计算值衍生出的值。

   如果你想响应式的产生一个可以被其它 observer 使用的值，请使用 `@computed`。

   `computed` 还可以直接当做函数来调用。 在返回的对象上使用 `.get()` 来获取计算的当前值，或者使用 `.observe(callback)` 来观察值的改变。 这种形式的 `computed` 不常使用，但在某些情况下，你需要传递一个“在box中”的计算值时，它可能是有用的。

   

4.  @action

   动作是用来修改状态的，应该永远只对修改状态的函数使用动作。

   建议对任何修改 observables 或具有副作用的函数使用 `@action` 。 结合开发者工具的话，动作还能提供非常有用的调试信息。

   `runInAction` 是个简单的工具函数，它接收代码块并在(异步的)动作中执行。这对于即时创建和执行动作非常有用，例如在异步过程中。`runInAction(f)` 是 `action(f)()` 的语法糖。

   

5.  例子

   ```
   import { observable, computed, action } from "mobx";
   import { observer } from 'mobx-react'

   @observer
   class testComponent extends React.Component {
       @observable num1 = 0;
       @observable num2 = 0;

       @computed get total() {
           return this.num1 + this.num2;
       }
       
       @action
       onChangeNum1 = (event) => {
   		this.num1 = event.target.value
   	}
   	
   	@action
       onChangeNum2 = (event) => {
   		this.num2 = event.target.value
   	}
   	
       render() {
   		return(
   			<div>	
   				Num1: <input type="text" onChange={this.onChangeNum1}/>
   				NUm2: <input type="text" onChange={this.onChangeNum2}/>
   				Show count: <span>{this.total}<span>
   			</div>
   		)
   	}
   }
   ```