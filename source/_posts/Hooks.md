---
layout: react
title: React Hooks
date: 2020-06-10 10:15:56
tags: React
categories: React
---

> **Hooks是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其它的 React 特性。**第一次接触Hooks是因为新项目要求使用函数组件和hooks，就开始了hooks的学习和使用。

## 学习hooks前的注意

1. **完全可选的。**无需重写任何已有代码就可以在一些组件中使用Hooks。若是用不到hooks，也不用去学习和使用它。
2. **100%向后兼容的。**Hook 不包含任何破坏性改动。
3. **现在可用。** Hook 已发布于 v16.8.0。
4. **没有计划从React中移除class。**
5. **Hooks 不会影响你对React概念的影响**。Hooks 为已知的 React 概念提供了更直接的 API：props， state，context，refs 以及生命周期。

## 组件类的缺点

React 的核心是组件。v16.8 版本之前，组件的标准写法是类（class）。一个很简单的按钮组件类就有很多行代码，代码看起来很重。真实的 React App 由多个类按照层级，一层层构成，复杂度成倍增长。再加入 Redux，就变得更复杂。

Redux 的作者 Dan Abramov [总结](https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889)了组件类的几个缺点。

- 大型组件很难拆分和重构，也很难测试。
- 业务逻辑分散在组件的各个方法之中，导致重复逻辑或关联逻辑。
- 组件类引入了复杂的编程模式，比如 render props 和高阶组件。

## 函数组件

React 团队希望，组件不要变成复杂的容器，最好只是数据流的管道。开发者根据需要，组合管道即可。 **组件的最佳写法应该是函数，而不是类。**

React 早就支持[函数组件](https://reactjs.org/docs/components-and-props.html)

但是，函数组件有重大限制，必须是纯函数，不能包含状态，也不支持生命周期方法，因此无法取代类。

**React Hooks 的设计目的，就是加强版函数组件，完全不使用"类"，就能写出一个全功能的组件。**

## Hooks 的含义

Hook 这个单词的意思是"钩子"。

**React Hooks 的意思是，组件尽量写成纯函数，如果需要外部功能和副作用，就用钩子把外部代码"钩"进来。** React Hooks 就是那些钩子。

你需要什么功能，就使用什么钩子。React 默认提供了一些常用钩子，你也可以封装自己的钩子。

所有的钩子都是为函数引入外部功能，所以 React 约定，钩子一律使用`use`前缀命名，便于识别。

## useState()：状态钩子

`useState()`用于为函数组件引入状态（state）。纯函数不能有状态，所以把状态放在钩子里面。

```React
import React from "react";

const Button = () => {
  const [buttonText, setButtonText] = React.useState("Click me,   please");

  const onChangeButtonText = () => {
    setButtonText("Thanks, been clicked!");
  }

  return  <button  onClick={onChangeButtonText}>{buttonText}</button>
}
```

Button 组件是一个函数，内部使用`useState()`钩子引入状态。

`useState()`这个函数接受状态的初始值，作为参数，上例的初始值为按钮的文字。该函数返回一个数组，数组的第一个成员是一个变量（上例是`buttonText`），指向状态的当前值。第二个成员是一个函数，用来更新状态，约定是`set`前缀加上状态的变量名（上例是`setButtonText`）。

## useEffect()：副作用钩子

`useEffect()`用来引入具有副作用（对环境的改变）的操作，最常见的就是向服务器请求数据。

作为componentDidMount()使用，第二个参数使用[]。

作为componentDidUpdate()使用，可在第二个参数数组中指定依赖。

作为componentWillUnMount()使用，通过return函数使用，组件要卸载的时候执行。

当多个useEffect存在时，按照顺序执行。

放在`componentDidMount`里面的代码，现在可以放在`useEffect()`。

```react
useEffect(()  =>  {
  // Async Action
}, [dependencies])
```

`useEffect()`接受两个参数。第一个参数是一个函数，异步操作的代码放在里面。第二个参数是一个数组，用于给出 Effect 的依赖项，只要这个数组发生变化，`useEffect()`就会执行。第二个参数可以省略，这时每次组件渲染时，就会执行`useEffect()`。

```react
const Person = ({ personId }) => {
  const [loading, setLoading] = React.useState(true);
  const [person, setPerson] = React.useState({});

  React.useEffect(() => {
    setLoading(true); 
    fetch(`/people/${personId}/`)
      .then(response => response.json())
      .then(data => {
        setPerson(data);
        setLoading(false);
      });
  }, [personId])

  if (loading === true) {
    return <p>Loading ...</p>
  }

  return <div>
    <p>You're viewing: {person.name}</p>
    <p>Height: {person.height}</p>
    <p>Mass: {person.mass}</p>
  </div>
}
```

上面代码中，每当组件参数`personId`发生变化，`useEffect()`就会执行。组件第一次渲染时，`useEffect()`也会执行。

## useContext()：共享状态钩子

如果需要在组件之间共享状态，可以使用`useContext()`。

第一步就是使用 React Context API，在组件外部建立一个 Context

//context.jsx

```javascript
const AppContext = React.createContext({});
```

//app.jsx

```react
<AppContext.Provider value={{
  username: 'superawesome'
}}>
  <div className="App">
    <Navbar/>
    <Messages/>
  </div>
</AppContext.Provider>
```

//navbar.jsx

```react
const Navbar = () => {
  const { username } = React.useContext(AppContext);
  return (
    <div className="navbar">
      <p>AwesomeSite</p>
      <p>{username}</p>
    </div>
  );
}
```

//message.jsx

```react
const Messages = () => {
  const { username } = React.useContext(AppContext)

  return (
    <div className="messages">
      <h1>Messages</h1>
      <p>1 message for {username}</p>
      <p className="message">useContext is awesome!</p>
    </div>
  )
}
```

## useRef(): 在组件render时不变的值

`useRef` 返回一个可变的 ref 对象，其 `.current` 属性被初始化为传入的参数（`initialValue`）。返回的 ref 对象在组件的整个生命周期内保持不变。

- useState/useReducer 每次render的时候state都会变。
- useMemo/useCallback 每次render的时候，依赖变了，state就会变。
- useRef, 每次render的时候都不会变。

React 16.3 发布React.createRef()，并且是现在类组件中推荐使用的。

Refs 是使用 `React.createRef()` 创建的，并通过 `ref` 属性附加到 React 元素。在构造组件时，通常将 Refs 分配给实例属性，以便可以在整个组件中引用它们。当 ref 被传递给 `render` 中的元素时，对该节点的引用可以在 ref 的 `current` 属性中被访问。

React 会在组件挂载时给 `current` 属性传入 DOM 元素，并在组件卸载时传入 `null` 值。`ref`会在 `componentDidMount` 或 `componentDidUpdate` 生命周期钩子触发前更新。

createRef 和 useRef 的用法：

```react
import React from "react";
function Search() {
  const inputRef = React.createRef();
  const onSearch = () => {
    const searchValue = inputRef.current.value
    //to search
    console.log(searchValue);
  };
  return (
    <div>
      <input type="text" placeholder="Search" ref={inputRef} />
      <button onClick={onSearch}>Search</button>
    </div>
  );
};
```

因为函数组件没有实例，如果想用ref获取子组件的实例，子组件组要写成类组件。

```react
import React, { Component, useEffect, useRef } from 'react';
function App() {
  const childRef = useRef();
  useEffect(() => {
    console.log('useRef')
    console.log(childRef.current)
    childRef.current.handleLog();
  }, [])
  return (
    <div>
      <h1>Hello World!</h1>
      <Child ref={childRef} count="1"/>
    </div>
  )
}
// 因为函数组件没有实例，如果想用ref获取子组件的实例，子组件组要写成类组件
class Child extends Component {
  handleLog = () => {
    console.log('Child Component');
  }
  render() {
    const { count } = this.props;
    return <h2>count: { count }</h2>
  }
}
export default App;
```

## useReducer()：action 钩子

React 本身不提供状态管理功能，通常需要使用外部库。这方面最常用的库是 Redux。

Redux 的核心概念是，组件发出 action 与状态管理器通信。状态管理器收到 action 以后，使用 Reducer 函数算出新的状态，Reducer 函数的形式是`(state, action) => newState`。

`useReducers()`钩子用来引入 Reducer 功能。

```react
const [state, dispatch] = useReducer(reducer, initialState);
```

上面是`useReducer()`的基本用法，它接受 Reducer 函数和状态的初始值作为参数，返回一个数组。数组的第一个成员是状态的当前值，第二个成员是发送 action 的`dispatch`函数。

下面是一个计数器的例子。用于计算状态的 Reducer 函数如下。

```react
const myReducer = (state, action) => {
  switch(action.type)  {
    case('countUp'):
      return  {
        ...state,
        count: state.count + 1
      }
    default:
      return  state;
  }
}
```

组件代码如下。

```react
function App() {
  const [state, dispatch] = useReducer(myReducer, { count:   0 });
  return  (
    <div className="App">
      <button onClick={() => dispatch({ type: 'countUp' })}>
        +1
      </button>
      <p>Count: {state.count}</p>
    </div>
  );
}
```

由于 Hooks 可以提供共享状态和 Reducer 函数，所以它在这些方面可以取代 Redux。但是，它没法提供中间件（middleware）和时间旅行（time travel），如果你需要这两个功能，还是要用 Redux。

## 创建自己的 Hooks

```react
const usePerson = (personId) => {
  const [loading, setLoading] = React.useState(true);
  const [person, setPerson] = React.useState({});
  useEffect(() => {
    setLoading(true);
    fetch(`/people/${personId}/`)
      .then(response => response.json())
      .then(data => {
        setPerson(data);
        setLoading(false);
      });
  }, [personId]);  
  return [loading, person];
};
```

上面代码中，`usePerson()`就是一个自定义的 Hook。

Person 组件就改用这个新的钩子，引入封装的逻辑。

```react
const Person = ({ personId }) => {
  const [loading, person] = usePerson(personId);

  if (loading === true) {
    return <p>Loading ...</p>;
  }

  return (
    <div>
      <p>You're viewing: {person.name}</p>
      <p>Height: {person.height}</p>
      <p>Mass: {person.mass}</p>
    </div>
  );
};
```

