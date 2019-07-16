---
title: JS中call和apply的区别与应用
date: 2016-09-10 11:44:30
tags: JavaScript
categories: 前端
---
## 一.方法定义
### 1、call()方法:
语法：call([thisObj[,arg1[, arg2[, [,.argN]]]]])

定义：调用一个对象的一个方法，以另一个对象替换当前对象。

参数:
thisObj可选项。将被用作当前对象的对象。
arg1, arg2, , argN
可选项。将被传递方法参数序列。

说明：
call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。
如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。


### 2、apply()方法：

语法：apply([thisObj[,argArray]])

定义：应用某一对象的一个方法，用另一个对象替换当前对象。

参数：thisObj
可选项。将被用作当前对象的对象。
argArray
可选项。将被传递给该函数的参数数组。

说明：如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj， 并且无法被传递任何参数。

## 二.call()和apply()相同点

a） 产生的效果或作用完全相同；

b） 至少有一个参数；

c） 第一个参数必须有且是一个对象（Object）。
## 三.call()和apply()的区别

传递参数的方式。用法上不同，主要是参数不完全同。
## 四.call()和apply()常用实例
### 实例一：

	function Animal(){    
	this.name = "Animal";    
	this.showName = function(){    
	    alert(this.name);    
	}    
	}    
	 
	function Cat(){    
	this.name = "Cat";    
	}    
	  
	var animal = new Animal();    
	var cat = new Cat();    
	
	//通过call或apply方法，将原本属于Animal对象的showName()方法交给对象cat来使用了。    
	//输入结果为"Cat"    
	animal.showName.call(cat,",");    
	//animal.showName.apply(cat,[]);  

注意：call 的意思是把 animal 的方法放到cat上执行，原来cat是没有showName() 方法，现在是把animal 的showName()方法放到 cat上来执行，所以this.name 应该是 Cat。

### 实例二：实现继承

	function Animal(name){      
	this.name = name;      
	this.showName = function(){      
	    alert(this.name);      
	}      
	}      

 	function Cat(name){    
    Animal.call(this, name);    
	}      
    
	var cat = new Cat("Black Cat");     
	cat.showName();  

注意：Animal.call(this) 的意思就是使用 Animal对象代替this对象，那么 Cat中不就有Animal的所有属性和方法了吗，Cat对象就能够直接调用Animal的方法以及属性了。