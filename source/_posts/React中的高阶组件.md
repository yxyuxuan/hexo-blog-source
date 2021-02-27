---
title: React中的高阶组件
date: 2020-07-16 15:18:55
tags: React
categories: React
---

> 为了提高组件的复用率，可测试性，就要保证组件功能单一性；但若要满足复杂需求就要扩展功能单一的组件，在React里就有了HOC（Higher-Order-Components）的概念。

定义：高阶组件是接受组件作为参数并且返回值为新组件的函数。简单来说高阶组件就是一个函数，传给它一个组件，它返回一个新的组件。

例子：

```javascript
import React, {Component} from 'react';

//高阶组件
const HOC = (wrappedComponent)=> 
	class WrapperComponent extends Component {
		render() {
			const newProps = {
				name: "HOC"
			}
			return(
				<wrappedComponent {...this.props} {...newProps}/>
			);
		}
	}
//普通组件
class WrappedComponent extends Component {
	render() {
		return (
			<div> hello world! </div>
		)
	}
}

export default HOC(WrappedComponent)
```

React是单向数据流，子组件对父组件透明，如果父组件想要获取子组件的状态，那么就需要用到ref。

ref的创建需用调用React.createRef()，然后将返回的对象作为子组件的props以ref传递：

```javascript
class ButtonComponent extends React.Component{
  render(){
    return <button>{this.props.children}</button>
  }
}
class App extends React.Component{
  ref = React.createRef();
  render(){
    return(
      <ButtonComponent ref={this.ref}>Click Me</FancyButton>
    )
  }
  componentDidMount() {
    console.log(this.ref.current);
  }
}
```

##### 总结:

高阶组件就是一个函数，传给它一个组件，它返回一个新的组件。新的组件使用传入的组件作为子组件。

高阶组件的作用是用于代码复用，可以把组件之间可复用的代码、逻辑抽离到高阶组件当中。新的组件和传入的组件通过 `props` 传递信息。

高阶组件有助于提高我们代码的灵活性，逻辑的复用性。灵活和熟练地掌握高阶组件的用法需要经验的积累还有长时间的思考和练习。