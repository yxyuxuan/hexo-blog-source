---
title: JavaScript中的Array数组
date: 2016-09-16 22:14:29
tags: JavaScript
categories: 前端
---


# 目录

1. 介绍：介绍 Array 数组对象的说明、定义方式以及属性。

2. 实例方法：介绍 Array 对象的实例方法：concat、every、filter、forEach、indexOf、join、lastIndexOf、map、pop、push、reverse、shift、slice、sort、splice、toString、tounshift等。

3. 静态方法：介绍 Array 对象的静态方法：Array.isArray()。

4. 实际操作：对 Array 进行示例操作：索引、for遍历、浅度复制、深度复制等操作。

 

## 1. 介绍

### 1.1 说明 

      数组是值的有序集合。每个值叫做一个元素，而每个元素在数组中有一个位置，以数字表示，称为索引。JavaScript数组是无类型：数组元素可以是任意类型，并且同一个数组中的不同元素也可能有不同的类型。 --《JavaScript权威指南(第六版)》

 


### 1.2 定义方式

var names = new Array("张三", "李四", "王五");
//或者
var names = ["张三", "李四", "王五"];


### 1.3 属性

length：表示数组内的元素长度。

 

## 2. 实例方法
常用方法：

	1) unshift() ：在数组头部插入元素
	
	2) shift() ：移除并返回数组的第一个元素
	
	3) push() ：在数组尾部插入元素
	
	4) pop() ：移除并返回数组的最后一个元素

### 2.1 concat() ：把元素衔接到数组中。不会修改原先的array,返回新的数组

参数：

	①value1,value2.....valueN ：任意多个值

返回值：

	{Array} 一个新的数组，包含原先的Array和新加入的元素。

示例：

	var demoArray = ['a', 'b', 'c'];
	var demoArray2 = demoArray.concat('e');
	console.log(demoArray); // => demoArray:['a','b','c']  原数组不发生变更
	console.log(demoArray2); // => ['a','b','c','e']


### 2.2 every() ：依次遍历元素，判断每个元素是否都为true

参数：

	function(value,index,self){} ：每个元素都会使用此函数判断是否为true，当判断到一个为false时，立即结束遍历。
	
	value ：数组遍历的元素
	
	index ：元素序号
	
	self ：Array本身

返回值：

	{Boolean} ：只有每个元素都为true才返回true；只要一个为false，就返回false。

示例：

	var demoArray = [1, 2, 3];
	var rs = demoArray.every(function (value, index, self) {
	return value > 0;
	});
	console.log(rs); // => true


### 2.3 filter() ：依次遍历元素，返回包含符合条件元素的新的数组

参数：

	function(value,index,self){} ：每个元素依次调用此函数，返回包含符合条件元素的新的数组。
	
	value ：数组遍历的元素
	
	index ：元素序号
	
	self ：Array本身

返回值：

    {Array} 一个包含符合条件元素的新的数组

示例：

    var demoArray = [1, 2, 3];
    var rs = demoArray.filter(function (value, index, self) {
    return value > 0;
    });
    console.log(rs); // => [1, 2, 3]


### 2.4 forEach() ：依次遍历元素，执行指定的函数；无返回值

参数：

	function(value,index,self){} ：每个元素依次调用此函数
	
	value ：数组遍历的元素
	
	index ：元素序号
	
	self ：Array本身

返回值：无

示例：

    var demoArray = [1, 2, 3];
    demoArray.forEach(function (value, index, self) {
    console.log(value); // => 依次输出：1  2  3
    });


### 2.5 indexOf() ：在数组中查找匹配元素。若不存在匹配的元素时，就返回-1。查找的时候使用"==="运算符，所以要区分1和'1' 

参数：

	①value ：要在数组中查找的值。
	
	②start ：开始查找的序号位置，如果省略，则为0.

返回值：

	{Int} ：返回数组中第一个匹配value的序号，若不存在，返回-1

示例：

	['a', 'b', 'c'].indexOf('a'); // =>0
	['a', 'b', 'c'].indexOf('a', 1); // =>-1
	['a', 'b', 'c'].indexOf('d'); // =>-1
	[1, 2, 3].indexOf('1'); // => -1 ：采用的'==='匹配方式


### 2.6 join() ：将数组中所有元素通过一个分隔符拼接为一个字符串

参数：

	①sparator {String}：各元素之间的分隔符，如果省略，默认以因为英文逗号','分隔。

返回值：

	{String} ：各元素以sparator为分隔符，拼接而成的一个字符串。

示例：

	['a', 'b', 'c'].join(); // => 'a,b,c'
	['a', 'b', 'c'].join('-'); // => 'a-b-c'


### 2.7 lastIndexOf ：在数组中反向查找匹配元素。若不存在匹配的元素时，就返回-1。查找的时候使用"==="运算符，所以要区分1和'1' 

参数：

	①value ：要在数组中查找的值。
	
	②start ：开始查找的序号位置，如果省略，则从最后一个元素开始查找。

返回值：

	{Int} ：从右到左开始查找数组中第一个匹配value的序号，若不存在，返回-1

示例：

	['a', 'b', 'c'].lastIndexOf('a'); // => 0
	['a', 'b', 'c'].lastIndexOf('a', 1); // => 0
	['a', 'b', 'c'].lastIndexOf('d'); // => -1
	[1, 2, 3].lastIndexOf('1'); // => -1 ：采用的'==='匹配方式


### 2.8 map() ：依次遍历并计算每个元素，返回计算好的元素的数组

参数：

	①function(value,index,self){} ：每个元素依次调用此函数，返回计算好的元素
	
	value ：数组遍历的元素
	
	index ：元素序号
	
	self ：Array本身

返回值：

	{Array} 一个包含就算好的元素的新的数组

示例：

    [1, 2, 3].map(function (value, index, self) {
    return value * 2;
    }); // => [2, 4, 6]


### 2.9 pop() ：移除并返回数组的最后一个元素

参数：无

返回值：

	{Object} 数组的最后一个元素；若数组为空，返回undefined

示例：

	var demoArray = ['a', 'b', 'c'];
	demoArray.pop(); // => c
	demoArray.pop(); // => b
	demoArray.pop(); // => a
	demoArray.pop(); // => undefined


### 2.10 push() ：把元素添加到数组尾部

参数：

	①value1,value2.....valueN ：任意多个值添加到数组尾部

返回值：

	{int} 数组新的长度 

示例：

	var demoArray = ['a', 'b', 'c'];
	demoArray.push('d'); // => 4, demoArray ： ['a', 'b', 'c', 'd']
	demoArray.push('e', 'f'); // => 6, demoArray ：['a', 'b', 'c', 'd', 'e', 'f']
	console.log(demoArray); // => ['a', 'b', 'c', 'd', 'e', 'f']


### 2.11 reverse() ：反转数组元素的顺序

参数：无

返回值：无(在原数组内进行元素顺序反转)。

示例：

	var demoArray = ['a', 'b', 'c', 'd', 'e'];
	demoArray.reverse();
	console.log(demoArray); // => ["e", "d", "c", "b", "a"]


###	2.12 shift() ：移除并返回数组的第一个元素

参数：无

返回值：

	{Object} 数组的第一个元素；若数组为空，返回undefined。

示例：

	var demoArray = ['a', 'b', 'c'];
	demoArray.shift(); // => a
	demoArray.shift(); // => b
	demoArray.shift(); // => c
	demoArray.shift(); // => undefined


### 2.13 slice(startIndex,endIndex) ：返回数组的一部分

参数：

	①startIndex ：开始处的序号；若为负数，表示从尾部开始计算，-1代表最后一个元素，-2倒数第二个，依此类推。
	
	②endIndex ： 结束处的元素后一个序号，没指定就是结尾。截取的元素不包含此处序号的元素，结尾为此处序号的前一个元素。

返回值：

	{Array} 一个新的数组，包含从startIndex到endIndex前一个元素的所有元素。

示例：

	[1, 2, 3, 4, 5, 6].slice(); // => [1, 2, 3, 4, 5, 6]
	[1, 2, 3, 4, 5, 6].slice(1); // => [2, 3, 4, 5, 6] ：从序号1开始截取
	[1, 2, 3, 4, 5, 6].slice(0, 4); // => [1, 2, 3, 4] ：截取序号0到序号3(序号4的前一个)的元素
	[1, 2, 3, 4, 5, 6].slice(-2); // => [5, 6] ：截取后面的2个元素


### 2.14 sort(opt_orderFunc) ：按一定的规则进行排序

参数：

	①opt_orderFunc(v1,v2) {Function}：可选的排序规则函数。若省略，将按照元素的字母进行从小到大排序。
	
	v1 ：遍历时前面的元素。
	
	v2 ：遍历时后面的元素。

排序规则：

	比较v1和v2，返回一个数字来表示v1和v2的排序规则：
	
	小于0 ：v1小于v2，v1排在v2的前面。
	
	等于0 ：v1等于v2，v1排在v2的前面。
	
	大于0 ：v1大于v2，v1排在v2的后面。

返回值：无(在原先数组里进行排序操作)。

示例：

	[1, 3, 5, 2, 4, 11, 22].sort(); // => [1, 11, 2, 22, 3, 4, 5] ：这里都元素都被转换为字符，11的字符在2前
	 
	[1, 3, 5, 2, 4, 11, 22].sort(function (v1, v2) {
	return v1 - v2;
	}); // => [1, 2, 3, 4, 5, 11, 22] ：从小到大排序
	 
	[1, 3, 5, 2, 4, 11, 22].sort(function (v1, v2) {
	return -(v1 - v2); //取反，就可以转换为 从大到小
	}); // => [22, 11, 5, 4, 3, 2, 1]


### 2.15 splice() ：插入、删除数组元素

参数：

	①start {int} ：开始插入、删除或替换的起始序号。
	
	②deleteCount {int} ：要删除元素的个数，从start处开始计算。
	
	③value1,value2 ... valueN {Object} ：可选参数，表示要插入的元素，从start处开始插入。若②参不为0，那么先执行删除操作，再执行插入操作。

返回值：

	{Array}  返回一个包含删除元素的新的数组。若②参为0，表示没元素删除，返回一个空数组。

示例：

	// 1.删除
	var demoArray = ['a', 'b', 'c', 'd', 'e'];
	var demoArray2 = demoArray.splice(0, 2); // 删除从序号从0开始的2个元素，返回包含删除元素的数组：['a', 'b']
	console.log(demoArray2); // => ['a', 'b']
	console.log(demoArray); // => ['c', 'd', 'e']
	 
	// 2.插入
	var demoArray = ['a', 'b', 'c', 'd', 'e'];
	var demoArray2 = demoArray.splice(0, 0, '1', '2', '3'); // ②参为0，返回空数组
	console.log(demoArray2); // => [ ]
	console.log(demoArray); // => ['1', '2', '3', 'a', 'b', 'c', 'd', 'e']
	 
	// 3.先删除再插入
	var demoArray = ['a', 'b', 'c', 'd', 'e'];
	// 当②参不为0，那么先执行删除操作(删除序号从0开始的4个元素，返回包含被删除元素的数组)，再执行插入操作
	var demoArray2 = demoArray.splice(0, 4, '1', '2', '3');
	console.log(demoArray2); // => ['a', 'b', 'c', 'd'] 
	console.log(demoArray); // => ['1', '2', '3', 'e']


### 2.16 toString() ：将数组中所有元素通过一个英文逗号','拼接为一个字符串

参数：无

返回值：

	{String}  数组中所有元素通过一个英文逗号','拼接为一个字符串，并返回。与调用无参join()方法一样。

示例：

	[1, 2, 3, 4, 5].toString(); // => '1,2,3,4,5'
	['a', 'b', 'c', 'd', 'e'].toString(); // => 'a,b,c,d,e'


### 2.17 unshift() ：在数组头部插入元素

参数：

	①value1,value2.....valueN ：任意多个值添加到数组头部

返回值：

	{int} 数组新的长度 

示例：

	var demoArray = [];
	demoArray.unshift('a'); // => demoArray:['a']
	demoArray.unshift('b'); // => demoArray:['b', 'a']
	demoArray.unshift('c'); // => demoArray:['c', 'b', 'a']
	demoArray.unshift('d'); // => demoArray:['d', 'c', 'b', 'a']
	demoArray.unshift('e'); // => demoArray:['e', 'd', 'c', 'b', 'a']


## 3. 静态方法

### 3.1 Array.isArray() ：判断对象是否为数组

参数：

	①value {Object}：任意对象

返回值：

	{Boolean}  返回判断结果。当为 true时，表示对象为数组；为false时，表示对象不是数组

示例：
	
	Array.isArray([]); // => true
	Array.isArray(['a', 'b', 'c']); // => true
	Array.isArray('a'); // => false
	Array.isArray('[1, 2, 3]'); // => false


## 4. 实际操作

### 4.1 索引

说明：

	每个元素在数组中有一个位置，以数字表示，称为索引。索引是从0开始计，即第一个元素的索引为0，第二个元素的索引为1，依此类推；
	
	    当获取一个数组不存在的索引时，返回 undefined。

示例：

	var demoArray = ['a', 'b', 'c', 'd', 'e'];
	demoArray[0]; // => 获取第一个元素:'a'
	demoArray[0] = 1;  // 设置第一个元素为 1
	console.log(demoArray); // => demoArray:[1, 'b', 'c', 'd', 'e']
	console.log(demoArray[9]); // => undefined ：当获取的索引不存在时，返回 undefined


### 4.2 for 语句

说明：

	可以通过for语句逐个遍历数组

示例：

	var demoArray = ['a', 'b', 'c', 'd', 'e'];
	for (var i = 0, length = demoArray.length; i < length; i++) {
	console.log(demoArray[i]); // => 逐个输出数组内的元素
	}


### 4.3 浅度复制

说明：

	Array类型是一种引用类型；当数组a复制给数组b时，对数组b进行元素修改，数组a也会发生修改。

示例：

	var demoArrayA = ['a', 'b', 'c', 'd', 'e'];
	var demoArrayB = demoArrayA; // 把数组A 赋值给数组B
	demoArrayB[0] = 1; // 对数组B 的元素进行修改
	console.log(demoArrayA); // => [1, 'b', 'c', 'd', 'e']：数组A 的元素也发生了变更


### 4.4 深度复制

说明：

	使用concat()方法，返回新的数组；防止浅度复制的情况发生，对数组b进行元素修改操作，数组a不发生变更。

示例：

	var demoArrayA = ['a', 'b', 'c', 'd', 'e'];
	var demoArrayB = demoArrayA.concat(); // 使用concat()方法，返回新的数组
	demoArrayB[0] = 1; // 对数组B 的元素进行修改
	console.log(demoArrayA); // => ['a', 'b', 'c', 'd', 'e']：数组A 的元素没变更
	console.log(demoArrayB); // => [  1, 'b', 'c', 'd', 'e']：数组B 的元素发生了变更


### 4.5 判断2个数组是否相等

说明：

	Array数组为引用类型，所以哪怕 []===[] 都会返回false，所以可通过数组toString()方法返回的字符串判断是否相等。

示例：

	console.log([]===[]); // => false
	console.log(['a', 'b'] === ['a', 'b']); // => false
	console.log(['a', 'b'].toString() === ['a', 'b'].toString()); // true