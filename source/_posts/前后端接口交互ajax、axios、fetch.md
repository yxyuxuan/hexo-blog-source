---
title: 数据请求之ajax、axios、fetch
date: 2020-03-01 15:36:23
tags: 前端
categories: 前端
---

> 对于搭建大型项目前后端分离是必须的，前后端分离的好处不用多说。前端工作一个重要的技能就是对接后台给的接口，从最初的Ajax，到目前比较火的fetch和axios，因为axios可以拦截请求和响应，转换请求数据和响应数据，并对响应回来的内容自动转换为json类型的数据等优势，个人比较喜欢使用axios。

## ajax

Ajax是一种可以在浏览器和服务器之间使用异步数据传输（HTTP请求）的技术。使用它可以让页面请求少量的数据，而不用刷新整个页面。而传统的页面（不使用Ajax）要刷新部分内容，必须重载整个网页页面。它基于的是XMLHttpRequest（XHR）。这是一个比较粗糙的API，不符合关注分离的设计原则（Separation of Concerns），配置和使用都不是那么友好。

## fetch

fetch是在ES6出现的，使用了ES6中的promise对象。Fetch是基于promise设计的。提供了一个 JavaScript接口，用于访问和操纵HTTP管道的部分，例如请求和响应。它还提供了一个全局`fetch()`方法，该方法提供了一种简单，合理的方式来跨网络异步获取资源。fetch不是ajax的进一步封装，而是原生js。

fetch不是ajax的进一步封装，而是原生js，没有使用XMLHttpRequest对象。

旧版本的浏览器不支持Promise，需要使用polyfill es6-promise。

```typescript
function handleResponse(res: Response): Promise<any> {
  if (res.status === 200) {
    return res.json()
  } else {
    return handleError(res)
  }
}
function buildQueryString(params: {} | undefined) {
  let qs = ''
  if (params) {
    qs = Object.keys(params)
      .filter(k => params[k] !== undefined && params[k] !== null)
      .map(k => `${encodeURIComponent(k)}=${encodeURIComponent(params[k])}`)
      .join('&')
  }

  return qs === '' ? '' : `?${qs}`
}

private getWith(path: string, params: {} | undefined, handler): Promise<any> {
    const token = ""
    let url = `${path}${buildQueryString(params)}`
    return fetch(url, {
      method: 'GET',
      headers: {
        Authorization: `Bearer ${token}`
      }
    }).then(handler)
}
  
private get(path, params: {} | undefined): Promise<any> {
  return this.getWith(path, params, handleResponse)
}
```

优点：

- 符合关注分离，没有将输入、输出和用事件来跟踪的状态混杂在一个对象里
- 更好更方便的写法
- 更加底层，提供的API丰富（request, response）
- 脱离了XHR，是ES规范里新的实现方式

缺点：

- fetchtch只对网络请求报错，对400，500都当做成功的请求，需要封装去处理
- fetch默认不会带cookie，需要添加配置项
- fetch不支持abort，不支持超时控制，使用setTimeout及Promise.reject的实现的超时控制并不能阻止请求过程继续在后台运行，造成了量的浪费
- fetch没有办法原生监测请求的进度，而XHR可以。

## axios

axios是一个基于promise的HTTP库，支持promise的所有API。

axios也是对原生XHR的一种封装，不过是Promise实现版本，用于浏览器和node.js的http客户端

例子：

```javascript
axios
.post(url, data, {
  headers: {
     Accept: "application/json",
     "Content-Type": "application/json"
  }
})
.then(function (response) {
    console.log(response);
})
.catch(function (error) {
    console.log(error);
});
```

优缺点：

- 从 node.js 创建 http 请求。
- 支持 Promise API。
- 提供了一些并发请求的接口（重要，方便了很多的操作）。
- 在浏览器中创建 XMLHttpRequests。
- 在 node.js 则创建 http 请求。（自动性强）
- 支持 Promise API。
- 支持拦截请求和响应。
- 转换请求和响应数据。
- 取消请求。
- 自动转换 JSON 数据。
- 客户端支持防止CSRF。
- 客户端支持防御 XSRF。

#### 为什么要用axios?

axios 是一个基于Promise 用于浏览器和 nodejs 的 HTTP 客户端，它本身具有以下特征：

- 从浏览器中创建 XMLHttpRequest
- 从 node.js 发出 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求和响应数据
- 取消请求
- 自动转换JSON数据
- 客户端支持防止CSRF/XSRF（防止CSRF:就是让你的每个请求都带一个从cookie中拿到的key, 根据浏览器同源策略，假冒的网站是拿不到你cookie中得key的，这样，后台就可以轻松辨别出这个请求是否是用户在假冒网站上的误导输入，从而采取正确的策略。）
- axios既提供了并发的封装，也没有fetch的各种问题，而且体积也较小，当之无愧现在最应该选用的请求的方式。