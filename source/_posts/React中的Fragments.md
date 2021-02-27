---
title: React中的Fragments
date: 2020-06-26 13:48:37
tags: React
categories: React
---

> React 中的一个常见模式是一个组件返回多个元素。Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。

```react
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}
```

可以使用一种新的，且更简短的语法来声明 Fragments，像空标签：

```react
class Columns extends React.Component {
  render() {
    return (
      <>        
        <td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

可以像使用任何其他元素一样使用 `<> </>`，但是它不支持 key 属性。若需使用key 属性可以使用<React.Fragment></React.Fragment>，`key` 是唯一可以传递给 `Fragment` 的属性。

```react
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // 没有`key`，React 会发出一个关键警告
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```