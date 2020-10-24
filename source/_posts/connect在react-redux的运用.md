---
title: '@connect在react-redux的运用'
date: 2020-09-01 20:00:24
tags: React
categories: React
---

> React结合Redux时需要用到中间件connect，＠connect是什么？

**＠connect是react中用装饰器来装饰connect的写法**

例如：

```javascript
import React from 'react';
import * as actionCreators from './actionCreators';
import { bindActionCreators } from 'redux';
import { connect } from 'react-redux';

function mapStateToProps(state) {
  return { todos: state.todos };
}

function mapDispatchToProps(dispatch) {
  return { actions: bindActionCreators(actionCreators, dispatch) };
}

class MyApp extends React.Component {
  // ...define your main app here
}

export default connect(mapStateToProps, mapDispatchToProps)(MyApp);
```

使用装饰器的写法：

```javascript
import React from 'react';
import * as actionCreators from './actionCreators';
import { bindActionCreators } from 'redux';
import { connect } from 'react-redux';

function mapStateToProps(state) {
  return { todos: state.todos };
}

function mapDispatchToProps(dispatch) {
  return { actions: bindActionCreators(actionCreators, dispatch) };
}

@connect(mapStateToProps, mapDispatchToProps)
export default class MyApp extends React.Component {
  // ...define your main app here
}
```

但是由于装饰器的兼容性问题我们需要使用babel来转换，安装babel插件

```javascript
yarn add @babel/plugin-proposal-decorators
```

package.json下的babel配置

```javascript
"babel": {
  "presets": [
    "react-app"
  ],
  "plugins": [
    [
      "@babel/plugin-proposal-decorators",
      {
        "legacy": true
      }
    ]
  ]
},
```

根目录下创建jsconfig.json解决编辑器报错问题

```javascript
{
  "compilerOptions": {
    "experimentalDecorators": true
  }
}
```