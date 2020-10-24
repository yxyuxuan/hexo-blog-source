---
title: CSS3中box-sizing属性
date: 2016-09-18 20:36:11
tags: CSS3
categories: CSS
---

**简介：**

box-sizing 属性允许您以特定的方式定义匹配某个区域的特定元素。

**语法：**

box-sizing：content-box | border-box 默认值：content-box

**取值：**

1. content-box，padding和border不被包含在定义的width和height之内。对象的实际宽度等于设置的width值和border、padding之和，即 ( Element width = width + border + padding )。此属性表现为标准模式下的盒模型。

2. border-box，padding和border被包含在定义的width和height之内。对象的实际宽度就等于设置的width值，即使定义有border和padding也不会改变对象的实际宽度，即 ( Element width = width )。此属性表现为怪异模式下的盒模型。

**示例：**

1. content-box:

	.test1{ box-sizing:content-box; width:200px; padding:10px; border:15px solid #eee; }


> 实际宽度为：200+（10+10）+（15+15）， 内容宽度为：200 

2. border-box:

	.test2{ box-sizing:border-box; width:200px; padding:10px; border:15px solid #eee; }


> 实际宽度为：200， 内容宽度为：200-（10+10）-（15+15）


**内核类型写法：**

	Webkit(Chrome/Safari)-webkit-box-sizing
	
	Gecko(Firefox)-moz-box-sizing
	
	Presto(Opera)-o-box-sizing
	
	Trident(IE)IE8:-ms-box-sizing/IE9:box-sizing