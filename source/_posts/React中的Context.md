---
title: React Context
date: 2020-06-22 18:18:05
tags: React
categories: React
---

> Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。



Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。

**使用props进行数据传递如下：**

```react
//App.js
class App extends React.Component {
  render() {
    return <Toolbar theme="dark" />;
  }
}
```

```react
//Toolbar.js
function Toolbar(props) {
  // Toolbar 组件接受一个额外的“theme”属性，然后传递给 ThemedButton 组件。  
  // 如果应用中每一个单独的按钮都需要知道 theme 的值，这会是件很麻烦的事，  
  // 因为必须将这个值层层传递所有组件。  
  return (    
    <div>
     	<ThemedButton theme={props.theme} />
   	</div> 
  );
}
```

```react
//ThemedButton.js
class ThemedButton extends React.Component {
  render() {
    return <Button theme={this.props.theme} />;
  }
}
```

**使用 context进行数据传递如下：**	

```react
//ThemeContext.js
//使用createContext为theme 创建一个context。
const ThemeContext = React.createContext({
    theme:"light"
});
```

```react
//App.js
import ThemeContext from "ThemeContext.js";

class App extends React.Component {
  render() {
    // 使用Provider将当前的theme的值传递给组件树，且通过value来改变theme的值。
    //接下来组件树接受的theme值为dark。    
    return (
      <ThemeContext.Provider value={{theme: "dark"}}>        
          <Toolbar />
      </ThemeContext.Provider>
    );
  }
}
```

```react
//Toolbar.js
//中间的组件不用向下传递theme了。
function Toolbar() {  
  return (
    <div>
      <ThemedButton />
    </div>
  );
}
```

```react
//ThemedButton.js
import ThemeContext from "ThemeContext.js";

function ThemedButton() {
  // 使用Consumer获取theme的值，此时theme的值为dark。 
  return (
    <ThemeContext.Consumer>
      {value=> <Button theme={value.theme} /> }
    </ThemeContext.Consumer>  
  ) 
}
```

**使用v16.8.0版本新增的Hooks中提出的useContext来更加便利的使用context：**

```react
//ThemedButton.js
import React from 'react';
import ThemeContext from "ThemeContext.js";

function ThemedButton() {
  // 使用useContext获取theme的值。 
  const value = React.useContext(ThemeContext);
  return (
     <Button theme={value.theme} /> 
  ) 
}
```

