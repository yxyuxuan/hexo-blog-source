---
title: Websocket
date: 2020-03-15 21:03:10
tags: 网络
categories: 网络
---

> WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。
>
> WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

### 传统轮询(Traditional Polling)

当前Web应用中较常见的一种持续通信方式，通常采取 setInterval 或者 setTimeout 实现。例如如果我们想要定时获取并刷新页面上的数据，可以结合Ajax写出如下实现：

```javascript
setInterval(function() {
    $.get("/path/to/server", function(data, status) {
        console.log(data);
    });
}, 10000);
```

上面的程序会每隔10秒向服务器请求一次数据，并在数据到达后存储。这个实现方法通常可以满足简单的需求，然而同时也存在着很大的缺陷：在网络情况不稳定的情况下，服务器从接收请求、发送请求到客户端接收请求的总时间有可能超过10秒，而请求是以10秒间隔发送的，这样会导致接收的数据到达先后顺序与发送顺序不一致。于是出现了采用 setTimeout 的轮询方式：

```javascript
function poll() {
    setTimeout(function() {
        $.get("/path/to/server", function(data, status) {
            console.log(data);
            // 发起下一次请求
            poll();
        });
    }, 10000);
}
```

程序首先设置10秒后发起请求，当数据返回后再隔10秒发起第二次请求，以此类推。这样的话虽然无法保证两次请求之间的时间间隔为固定值，但是可以保证到达数据的顺序。

### 缺陷

程序在每次请求时都会新建一个HTTP请求，然而并不是每次都能返回所需的新数据。当同时发起的请求达到一定数目时，会对服务器造成较大负担。

### 长轮询(long poll)

客户端发送一个request后，服务器拿到这个连接，如果有消息，才返回response给客户端。没有消息，就一直不返回response。之后客户端再次发送request, 重复上次的动作。

### 总结

http协议的特点是服务器不能主动联系客户端，只能由客户端发起。它的被动性预示了在完成双向通信时需要不停的连接或连接一直打开，这就需要服务器快速的处理速度或高并发的能力，是非常消耗资源的。

### 什么是websocket?

WebSocket是HTML5的一个新协议，它允许服务端向客户端传递信息，实现浏览器和客户端双工通信

因为 HTTP 协议有一个缺陷：通信只能由客户端发起。 举例来说，我们想了解今天的天气，只能是客户端向服务器发出请求，服务器返回查询结果。HTTP 协议做不到服务器主动向客户端推送信息。这种单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。我们只能使用"轮询"：每隔一段时候，就发出一个询问,轮询的效率低，非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）。因此，工程师们一直在思考，有没有更好的方法。WebSocket 就是这样发明的。

### Websocket的特点:

服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话，属于服务器推送技术的一种。

- 与 HTTP 协议有着良好的兼容性。默认端口也是 80 和 443 ，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。 
- 建立在TCP协议基础之上，和http协议同属于应用层
- 数据格式比较轻量，性能开销小，通信高效。 
- 可以发送文本，也可以发送二进制数据。 
- 没有同源限制，客户端可以与任意服务器通信
- 协议标识符是ws（如果加密，则为wss），服务器网址就是 URL,如ws://localhost:8023

### 跨平台的WebSocket通信库socket.io

跨平台的WebSocket通信库，具有前后端一致的API，可以触发和响应自定义的事件。socket.io最核心的两个api就是emit 和 on了 ，服务端和客户端都有这两个api。通过 emit 和 on可以实现服务器与客户端之间的双向通信。

- emit ：发射一个事件，第一个参数为事件名，第二个参数为要发送的数据，第三个参数为回调函数（如需对方接受到信息后立即得到确认时，则需要用到回调函数）。 
- on ：监听一个 emit 发射的事件，第一个参数为要监听的事件名，第二个参数为回调函数，用来接收对方发来的数据，该函数的第一个参数为接收的数据。

### 服务端

```javascript
var app = require('express')();
var http = require('http');
var socketio  = require("socket.io");
const server = http.createServer(app)
const io = socketio(server)
var count = 0;
// WebSocket 连接服务器
io.on('connection', (socket)=> {
    //// 所有的事件触发响应都写在这里
    setInterval(()=>{
        count++
        //向建立该连接的客户端发送消息
        socket.emit('mynameEv', { name:"你我贷"+count})
    },1000)
    //监听客户端发送信息
    socket.on('yournameEv', function (data) {
        console.log(data)
    })
})

app.get('/', function (req, res) {
    res.sendfile(__dirname + '/index.html');
});
// 启用3000端口
server.listen(3000)
```

### 客户端

```javascript
<body>
   <div id="myname"></div>
   <script src="http://localhost:3000/socket.io/socket.io.js"/>
   <script>
      var count = 0;
      const socket = io.connect('http://localhost:3000')
      socket.on('mynameEv', (data)=>{
          document.getElementById("myname").innerHTML = data.name;
         console.log(data.name)
         setInterval(()=>{
                count++
                socket.emit('yournameEv', { name:"飞旋"+count})
         },1000)

      })
   </script>
</body>
```