---
title: Node.js中的events模块
date: 2016-10-23 21:16:55
tags: Node.js
categories: 前端
---


 **注意：**

1. Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。

2. Node.js里面的许多对象都会分发事件：一个net.Server对象会在每次有新连接时分发一个事件， 一个fs.readStream对象会在文件被打开的时候发出一个事件。 所有这些产生事件的对象都是 events.EventEmitter 的实例。

----------

### 一.EventEmitter类

events 模块只提供了一个对象： events.EventEmitter。EventEmitter 的核心就是事件触发与事件监听器功能的封装。可以通过require("events");来访问该模块。

    // 引入 events 模块
    var events = require('events');
    // 创建 eventEmitter 对象
    var eventEmitter = new events.EventEmitter();

EventEmitter 对象如果在实例化时发生错误，会触发 'error' 事件。当添加新的监听器时，'newListener' 事件会触发，当监听器被移除时，'removeListener' 事件被触发。

### 二.EventEmitter 的用法：

    //event.js 文件
    var EventEmitter = require('events').EventEmitter; 
    var event = new EventEmitter(); 
    event.on('some_event', function() { 
        console.log('some_event 事件触发'); 
    }); 
    setTimeout(function() { 
        event.emit('some_event'); 
    }, 1000); 

执行结果如下：
运行这段代码，1 秒后控制台输出了 'some_event 事件触发'。其原理是 event 对象注册了事件 some_event 的一个监听器，然后我们通过 setTimeout 在 1000 毫秒以后向 event 对象发送事件 some_event，此时会调用some_event 的监听器。

    $ node event.js 
    some_event 事件触发

EventEmitter 的每个事件由一个事件名和若干个参数组成，事件名是一个字符串，通常表达一定的语义。对于每个事件，EventEmitter 支持 若干个事件监听器。

当事件触发时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递。
让我们以下面的例子解释这个过程：

    //event.js 文件
    var events = require('events'); 
    var emitter = new events.EventEmitter(); 
    emitter.on('someEvent', function(arg1, arg2) { 
        console.log('listener1', arg1, arg2); 
    }); 
    emitter.on('someEvent', function(arg1, arg2) { 
        console.log('listener2', arg1, arg2); 
    }); 
    emitter.emit('someEvent', 'arg1 参数', 'arg2 参数'); 

执行以上代码，运行的结果如下：

    $ node event.js 
    listener1 arg1 参数 arg2 参数
    listener2 arg1 参数 arg2 参数

以上例子中，emitter 为事件 someEvent 注册了两个事件监听器，然后触发了 someEvent 事件。
运行结果中可以看到两个事件监听器回调函数被先后调用。 这就是EventEmitter最简单的用法。

### 三.EventEmitter 的属性

 EventEmitter 提供了多个属性，如 on 和 emit。on 函数用于绑定事件函数，emit 属性用于触发一个事件。接下来我们来具体看下 EventEmitter 的属性介绍。

1. addListener(event, listener)
为指定事件添加一个监听器到监听器数组的尾部。

2. on(event, listener)
为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数。

        erver.on('connection', function (stream) {
    		console.('someone connected!');
    	});
  
3. once(event, listener)
为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器。

    	server.once('connection', function (stream){
    		console.log('Ah, we have our first user!');
    	});

4. removeListener(event, listener)
移除指定事件的某个监听器，监听器 必须是该事件已经注册过的监听器。

		var callback = function(stream) {
  	 		console.log('someone connected!');
    	};
    	server.on('connection', callback);
    	// ...
    	server.removeListener('connection', callback);
  
5. removeAllListeners([event])
移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。
6. setMaxListeners(n)
默认情况下， EventEmitters 如果你添加的监听器超过 10 个就会输出警告信息。 setMaxListeners 函数用于提高监听器的默认限制的数量。

7. listeners(event)
返回指定事件的监听器数组。

8. emit(event, [arg1], [arg2], [...])
按参数的顺序执行每个监听器，如果事件有注册监听返回 true，否则返回 false。

### 四.继承 EventEmitter


- 大多数时候我们不会直接使用 EventEmitter，而是在对象中继承它。包括 fs、net、 http 在内的，只要是支持事件响应的核心模块都是 EventEmitter 的子类。



- 为什么要这样做呢？原因有两点：
首先，具有某个实体功能的对象实现事件符合语义， 事件的监听和发射应该是一个对象的方法。
其次 JavaScript 的对象机制是基于原型的，支持 部分多重继承，继承 EventEmitter 不会打乱对象原有的继承关系。
