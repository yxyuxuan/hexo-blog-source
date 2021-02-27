---
title: 用Node.js实现HTTP服务器程序
date: 2016-10-21 20:57:26
tags: Node.js
---


> 最近在学习Node.js的基本模块,现用Node.js实现一个HTTP服务器程序

**注意：**

1. 要开发HTTP服务器程序，从头处理TCP连接，解析HTTP是不现实的。这些工作实际上已经由Node.js自带的http模块完成了。应用程序并不直接和HTTP协议打交道，而是操作http模块提供的request和response对象。

2. request对象封装了HTTP请求，我们调用request对象的属性和方法就可以拿到所有HTTP请求的信息；

3. response对象封装了HTTP响应，我们操作response对象的方法，就可以把HTTP响应返回给浏览器。

----------

### 一.让我们先了解下 Node.js 应用是由哪几部分组成的：
1. **引入 required 模块：**我们可以使用 require 指令来载入 Node.js 模块。

2. **创建服务器**：服务器可以监听客户端的请求，类似于 Apache 、Nginx 等 HTTP 服务器。

3. **接收请求与响应请求:**服务器很容易创建，客户端可以使用浏览器或终端发送 HTTP 请求，服务器接收请求后返回响应数据。

### 二.Node.js实现一个HTTP服务器程序的步骤：

#### 1. 引入requried模块：

我们使用 require 指令来载入 http 模块，并将实例化的 HTTP 赋值给变量 http，实例如下:

    var http = require("http");

### 2. 创建服务器，接收请求与响应请求：
接下来我们使用 http.createServer() 方法创建服务器，并使用 listen 方法绑定 8080 端口。 函数通过 request, response 参数来接收和响应数据。实例如下：

    // 创建http server，并传入回调函数:
    var server = http.createServer(function (request, response) {
          
          // 将HTTP响应200写入response, 同时设置Content-Type: text/html:
          response.writeHead(200, {'Content-Type': 'text/html'});
    
          // 将HTTP响应的HTML内容写入response:
          response.end('<h1>Hello world!</h1>');
    });
    
    // 让服务器监听8080端口:
    server.listen(8080,'127.0.0.1');
    
    //// 终端打印如下信息
    console.log('Server is running at http://127.0.0.1:8080/');

### 3. 在命令提示符下运行该程序，可以看到以下输出：

    $ node hello.js 
    Server is running at http://127.0.0.1:8080/
### 4. 不要关闭命令提示符，直接打开浏览器输入http://127.0.0.1:8080，即可看到服务器响应的内容：

    Hello world!


## 三.分析Node.js 的 HTTP 服务器


- 第一行请求（require）Node.js 自带的 http 模块，并且把它赋值给 http 变量。


- 接下来我们调用 http 模块提供的函数： createServer 。这个函数会返回 一个对象，这个对象有一个叫做 listen 的方法，这个方法有一个数值参数， 指定这个 HTTP 服务器监听的端口号。

