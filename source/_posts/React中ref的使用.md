---
title: React中ref的使用
date: 2018-03-17 13:21:35
tags: React
categories: React
---

> 第一次接触ref是看到同事写的代码中用了ref，感觉好神奇，竟然可以获取到input的输入值，后来因为有次需要获取子组件中的数据，找来老大帮忙，他也用了ref,这次是用在组件上。今天整理了一些关于ref的内容，写了这篇博客。



​	在react典型的数据流(自顶向下传递）中`props`传递是父子组件交互的唯一方式；通过传递一个新的`props`值来使子组件重新`re-render`,从而达到父子组件通信。在react典型的数据量之外，某些情况下（和第三方的dom库整合，或者某个dom元素focus等）为了修改子组件我们可能需要另一种方式，这就是`ref`方式。

​	在React中，数据是自顶向下流动的（称为单项数据流），从父组件传递到子组件。因此组件是简单且易于把握的，它们只需从父节点获取**props**渲染即可。如果顶层组件的某个**prop**改变了，React会递归向下遍历整个组件树，从新渲染所有使用这个属性的组件。如果想要修改组件内部的状态使用**state** 。



#### **通过ref可以拿到DOM元素的值，并且能够对DOM元素进行操作**

```
class InputComponent extends React.Component {
  componentDidMount() {
    this.textInput.focus() //加载页面时对input输入框focus
  }

  getInputValue = () => {
    console.log(this.textInput.value) //点击按钮输出input的value值
  }

  render() {
    return (
      <div>
        <input type="text" ref={input => (this.textInput = input)} />
        <button onClick={this.getInputValue}>Get Input Value</button>
      </div>
    )
  }
}
```



#### **父组件通过ref可以拿到子组件的state值**

```
class InputBoxComponent extends React.Component {
  onClick = () => {
    const inputValue = this.textInputBox.state.inputValue
    console.log(inputValue) //输出iput输入框的值
  }

  render() {
    return (
      <div>
        <InputComponent ref={input => (this.textInputBox = input)} />
        <button onClick={this.onClick}>Get Input Value</button>
      </div>
    )
  }
}

class InputComponent extends React.Component {
  state = {
    inputValue: ''
  }

  onChangeInputValue = event => {
    this.setState({ ...this.state, inputValue: event.target.value })
  }

  render() {
    return <input type="text" onChange={this.onChangeInputValue} />
  }
}
```



​	`ref`提供了一种对于react标准的数据流不太适用的情况下组件间交互的方式，例如管理dom元素focus、text selection以及与第三方的dom库整合等等。 但是在大多数情况下应该使用react响应数据流那种方式，不要过度使用ref。

​	在使用ref时，不用担心会导致内存泄露的问题，react会自动帮你管理好，在组件卸载时ref值也会被销毁。