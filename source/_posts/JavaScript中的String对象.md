---
title: JavaScript中的String对象
date: 2016-09-16 22:23:56
tags: JavaScript
categories: JavaScript
---


## String的描述

	字符串是JavaScript中的一种基本的数据类型。
	String 对象的length属性声明了该字符串中的字符数。
	String 类定义了大量操作字符串的方法，例如从字符串中提取字符或子串，或者检索字符或子串。
	String 对象用于处理文本（字符串）。

**创建 String 对象的语法：**

	new String(s);
	String(s);

**参数**

	参数 s 是要存储在 String 对象中或转换成原始字符串的值。

**返回值**

	当 String() 和运算符 new 一起作为构造函数使用时，它返回一个新创建的 String 对象，存放的是字符串 s 或 s 的字符串表示。
	当不用 new 运算符调用 String() 时，它只把 s 转换成原始的字符串，并返回转换后的值。
## JavaScript 中 slice 、substr 和 substring的区别：
1. String.slice(start,end): 一个新的字符串。包括字符串 stringObject 从 start 开始（包括 start）到 end 结束（不包括 end）为止的所有字符.

2. String.substring(start,end) 这个就有点特别了，它是先从start，end里找出一个较小的值. 然后从字符串的开始位置算起，截取较小值位置和较大值位置之间的字符串,截取出来的字符串的长度为较大值与较小值之间的差。
一个新的字符串，该字符串值包含 stringObject 的一个子字符串，其内容是从 start 处到 stop-1 处的所有字符，其长度为 stop 减 start。

3. String.substr(start,end) 这个就是我们常用的从指定的位置(start)截取指定长度(end)的字符串。
一个新的字符串，包含从 stringObject 的 start（包括 start 所指的字符） 处开始的 lenght 个字符。如果没有指定 lenght，那么返回的字符串包含从 start 
到 stringObject 的结尾的字符。
String 对象的方法 slice()、substring() 和 substr() (不建议使用)都可返回字符串的指定部分。slice() 比 substring() 要灵活一些，因为它允许使用负数作为参数。slice() 与 substr() 有所不同，因为它用两个字符的位置来指定子串，而 substr() 则用字符位置和长度来指定子串。

## String对象中常用的方法：

#### 1. charCodeAt方法返回一个整数，代表指定位置字符的Unicode编码。

用法：

	strObj.charCodeAt(index)

说明：

	index将被处理字符的从零开始计数的编号。有效值为0到字符串长度减1的数字。
	如果指定位置没有字符，将返回NaN。
例如：

	var str = "ABC";
	str.charCodeAt(0);
	结果：65

#### 2. fromCharCode方法从一些Unicode字符串中返回一个字符串。

用法：

	String.fromCharCode([code1[,code2...]])
说明：

	code1，code2...是要转换为字符串的Unicode字符串序列。如果没有参数，结果为空字符串。
例如：

	String.fromCharCode(65,66,112);
	结果：ABp

#### 3. charAt方法返回指定索引位置处的字符。如果超出有效范围的索引值返回空字符串。

用法：

	strObj.charAt(index)
说明：

	index想得到的字符的基于零的索引。有效值是0与字符串长度减一之间的值。
例如：

	var str = "ABC";
	str.charAt(1);
	结果：B

#### 4. slice方法返回字符串的片段。

用法：

	strObj.slice(start[,end])
说明：

	start下标从0开始的strObj指定部分其实索引。如果start为负，将它作为length+start处理，此处length为字符串的长度。
	end小标从0开始的strObj指定部分结束索引。如果end为负，将它作为length+end处理，此处length为字符串的长度。
例如：

	var str = "ABCDEF";
	str.slice(2,4);
	结果：CD

#### 5. substring方法返回位于String对象中指定位置的子字符串。

用法：

	strObj.substring(start,end)
说明：

	start指明子字符串的起始位置，该索引从0开始起算。
	end指明子字符串的结束位置，该索引从0开始起算。
	substring方法使用start和end两者中的较小值作为子字符串的起始点。如果start或end为NaN或者为负数，那么将其替换为0。
例如：

	var str = "ABCDEF";
	str.substring(2,4); // 或 str.substring(4,2);
	结果：CD

#### 6. substr方法返回一个从指定位置开始的指定长度的子字符串。

用法：

	strObj.substr(start[,length])
说明：

	start所需的子字符串的起始位置。字符串中的第一个字符的索引为0。
	length在返回的子字符串中应包括的字符个数。
例如：

	var str = "ABCDEF";
	str.substr(2,4);
	结果：CDEF
#### 7. indexOf方法返回String对象内第一次出现子字符串位置。如果没有找到子字符串，则返回-1。
用法：

	strObj.indexOf(substr[,startIndex])
说明：

	substr要在String对象中查找的子字符串。
	startIndex该整数值指出在String对象内开始查找的索引。如果省略，则从字符串的开始处查找。
例如：

	var str = "ABCDECDF";
	str.indexOf("CD"，1); // 由1位置从左向右查找 123...
	结果：2
#### 8. lastIndexOf方法返回String对象中字符串最后出现的位置。如果没有匹配到子字符串，则返回-1。
用法：

	strObj.lastIndexOf(substr[,startindex])
说明：

	substr要在String对象内查找的子字符串。
	startindex该整数值指出在String对象内进行查找的开始索引位置。如果省略，则查找从字符串的末尾开始。
例如：

	var str = "ABCDECDF";
	str.lastIndexOf("CD",6); // 由6位置从右向左查找 ...456
	结果：5

#### 9. search方法返回与正则表达式查找内容匹配的第一个字符串的位置。
用法：

	strObj.search(reExp)
说明：

	reExp包含正则表达式模式和可用标志的正则表达式对象。
例如：

	var str = "ABCDECDF";
	str.search("CD"); // 或 str.search(/CD/i);
	结果：2
#### 10. concat方法返回字符串值，该值包含了两个或多个提供的字符串的连接。
用法：

	str.concat([string1[,string2...]])
说明：

	string1，string2要和所有其他指定的字符串进行连接的String对象或文字。
例如：

	var str = "ABCDEF";
	str.concat("ABCDEF","ABC");
	结果：ABCDEFABCDEFABC

#### 11. 将一个字符串分割为子字符串，然后将结果作为字符串数组返回。
用法：

	strObj.split([separator[,limit]])
说明：

	separator字符串或 正则表达式 对象，它标识了分隔字符串时使用的是一个还是多个字符。如果忽略该选项，返回包含整个字符串的单一元素数组。
	limit该值用来限制返回数组中的元素个数。
例如：

	var str = "AA BB CC DD EE FF";
	alert(str.split(" "，3));
	结果：AA,BB,CC

#### 12. toLowerCase方法返回一个字符串，该字符串中的字母被转换成小写。
例如：

	var str = "ABCabc";
	str.toLowerCase();
	结果：abcabc

#### 13. toUpperCase方法返回一个字符串，该字符串中的所有字母都被转换为大写字母。
例如：

	var str = "ABCabc";
	str.toUpperCase();
	结果：ABCABC



> 虽然js String对象已经提供像slice、replace、indexOf和substring等方法,但在实际项目应用中会对其进行扩展，以达到实用、方便目的。注释很详细，废话少说，代码如下：

## String对象方法扩展：



> 字符串-格式化

	String.prototype.format = function(){
	var args = arguments;//获取函数传递参数数组,以便在replace回调函数内使用
	var regex = /\{(\d+)\}/g;//匹配并捕获所有 形如:{数字} 字串
	return this.replace(regex,function(m,i){//参数=匹配子串+第几次匹配+匹配字串位置+源字符串
	return args[i];
	});
	}

> 字符串-去掉前后空白字符

	String.prototype.trim = function(){
	return this.replace(/(^\s*)|(\s*$)/g, "");
	}



> 字符串-去掉前空白字符

	String.prototype.ltrim = function(){
	return this.replace(/(^\s*)/g, "");
	}

> 字符串-去掉后空白字符

	String.prototype.rtrim = function(){
	return this.replace(/(\s*$)/g, "");
	}

> 字符串-获取以ASCII编码字节数 英文占1字节 中文占2字节

	String.prototype.lenASCII=function(){
	return this.replace(/[^\x00-\xff]/g,'xx').length;//将所有非\x00-\xff字符换为xx两个字符,再计算字符串
	}


> 字符串-获取以UNICODE编码字节数 一个字符均占2个字节

	String.prototype.lenUNICODE=function(){
	return this.length*2;
	}
	ps：若对js提供类型对象或自定义对象进行方法扩展，应利用原型prototype这个对象属性进行扩展，具体方式以下:
	
	String.prototype.trim=function(){
	//...代码略
	};
	String.prototype.ltrim=function(){
	//...代码略
	};