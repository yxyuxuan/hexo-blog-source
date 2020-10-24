---
title: React的事件机制
date: 2020-05-24 16:50:40
tags: React
categories: React
---

> React自身实现了一套自己的事件机制，包括事件注册、事件的合成、事件冒泡、事件派发等，虽然和原生的是两码事，但也是基于浏览器的事件机制下完成的。

React 的所有事件并没有绑定到具体的dom节点上而是绑定在了document 上，然后由统一的事件处理程序来处理，同时也是基于浏览器的事件机制（冒泡），所有节点的事件都会在 document 上触发。

```react
import React from "react";
class Test extends React.Component{
	componentDidMount(){ 
		document.getElementById('btn-reactandnative').addEventListener('click', (e) => {
            console.log('原生+react 事件:原生事件执行');
        });
    }

    handleNativeAndReact = (e) => {
        console.log('原生+react 事件:当前执行react事件');
    }

    handleClick=(e)=>{
        console.log('button click');
    }
    render(){
        return( 
            <div className="pageIndex">
                <p>react event!!!</p>
                <button id="btn-confirm" onClick={this.handleClick}>react事件</button>
                <button id="btn-reactandnative" onClick={this.handleNativeAndReact}>原生 + react 事件</button>
            </div>
		)
    }
}
```

`btn#btn-confirm `绑定了合成事件，`btn#btn-reactandnative`绑定了原生的事件。

只有原生的事件才会被绑到button上，合成事件在document上，由 dispatchEvent 统一去处理。



React中所有事件均注册到了元素的最顶层-document 上 ，节点的事件由统一的入口处理。优点如下：

1. 减少内存消耗，提升性能，不需要注册那么多的事件了，一种事件类型只在 document 上注册一次
2. 统一规范，解决 ie 事件兼容问题，简化事件逻辑
3. 对开发者友好



React v17 中，React 不会再将事件处理添加到 `document` 上，而是将事件处理添加到渲染 React 树的根 DOM 容器中：

```
const rootNode = document.getElementById('root');
ReactDOM.render(<App />, rootNode);
```

在 React 16 及之前版本中，React 会对大多数事件进行 `document.addEventListener()` 操作。React v17 开始会通过调用 `rootNode.addEventListener()` 来代替。

