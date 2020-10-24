---
title: React注意点
date: 2016-10-19 20:10:40
tags: React
categories: React
---

>**学习React时，碰到一些需要注意的地方。现整理如下：**

### 一.关于JSX 语法

React 不是一定要使用 JSX 语法，可以直接使用原生 JS。JSX语法看上去像是在Javascript代码里直接写起了XML标签，实质上这只是一个语法糖，每一个XML标签都会被JSX转换工具转换成纯Javascript代码，所以建议使用 JSX 是因为它能精确定义和反应组件及属性的树状结构，使得组件的结构和组件之间的关系看上去更加清晰。方便MXML和XAML的开发人员 – 因为他们已经使用过类似的语法。

### 二.HTML 标签 和 React 组件

在JSX语法中，遇到HTML标签（以<开头）就用HTML规则解析，遇到代码块（以{开头）就用JavaScript规则解析。React 可以渲染 HTML 标签 (strings) 或 React 组件 (classes)。

要渲染 HTML 标签，只需在 JSX 里使用小写字母开头的标签名。

     var myDivElement=<div className="foo"/>;
    
     React.render(myDivElement,document.body);

要渲染 React 模块，只需创建一个大写字母开头的本地变量。

    var MyComponent = React.createClass({/*...*/});
    var myElement = <MyComponent someProperty={true} />;
    React.render(myElement, document.body);
    React 的 JSX 里约定分别使用首字母大、小写来区分本地模块的类和 HTML 标签。

### 三.不建议作为 XML 属性名

由于 JSX 就是 JavaScript，一些标识符像 class 和 for 不建议作为 XML 属性名。作为替代，React DOM 使用 className 和 htmlFor 来做对应的属性。

### 四.大小写敏感

上面说了JSX是一个XML语法的预处理器。 XML 语法对大小写敏感，所以习惯了HTML的同学要特别注意这点，否则折腾了半天，都不知道错在哪里。比如：

    var Events = React.createClass({
        clickHandler: function(){
            console.log('You got me!');
        },
        render: function(){
          return <div onClick={this.clickHandler}>Hello, world!</div>;
        }
    });
     
    React.render( <Events />,document.body);
这里绑定click事件的onClick中C是大写的。

### 五.React 注释特点

1. jsx 标签内容注释 <p name="123">hello{// 或 /**/}</p>
2. jsx 标签属性注释 <p name="123"{// /**/}>123</p>
3. 其实上面提到的大括号的内容都是js 可以参考求值表达式

### 六.React Css样式与标签嵌套

1. 定义方式与定义对象一样。 css的写法少有变化，属性值需要加引号，属性与属性之间使用逗号分隔，而不在是分号例如

     var style={color:"red", border:"1px #000 solid"}

2. 样式的引用，React采用style的方式来引用样式；例如

     <div style={style}><HelloMessage /></div>

