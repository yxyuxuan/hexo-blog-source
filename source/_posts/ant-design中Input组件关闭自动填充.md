---
title: ant-design的Input组件关闭自动填充
date: 2020-08-29 14:17:56
tags: Ant Design
categories: 前端
---

> 默认情况下，浏览器会记录用户网页上提交的[输入](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)框的信息。这使得浏览器能够提供自动补全（在用户开始输入的时候给用户提供可能的内容）和自动填充（在加载的时候预先填充某些字段）功能。
>
> 这些功能通常是默认启用的，但可能涉及用户的隐私，因此浏览器允许用户禁用这些功能。

<input>元素关闭自动填充效果可以使用autocomplete=“off"来实现。

在Ant Design中Input组件中设置autocomplete=“off"是无效的，需要c大写，写成autoComplete=“off”有效。

例：

```jsx
import React from "react";
import {Input} from "antd";

class Example extends React.Component {
	render() {
        return(
        	<div>
            	<input autocomplete="off"/>
            	<Input autoComplete="off"/>
            </div>
        )
    }
}
```

