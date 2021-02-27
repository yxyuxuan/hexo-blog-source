---
title: Node.js中的fs模块
date: 2016-10-21 22:14:51
tags: Node.js
categories: 前端
---


>Node.js内置的fs模块就是文件系统模块，负责读写文件。和所有其它JavaScript模块不同的是，fs模块同时提供了异步和同步的方法。

### 一.同步读文件

1. 创建一个文件 input.txt ，内容如下：

    	你好，我是于禤。
        	Hello!my name is yuxuan.
2. 创建 main.js 文件, 代码如下：

    	var fs = require("fs");

        	var data = fs.readFileSync('input.txt');
    
        	console.log(data.toString());
        	console.log("程序执行结束!");
3. 以上代码执行结果如下：

    	$ node main.js
        
     	你好，我是于禤。
    		Hello!my name is yuxuan.
    		程序执行结束!
     
### 二.异步读文件
1. 创建一个文件 input.txt ，内容如下：

    	你好，我是于禤。
      	 	Hello!my name is yuxuan.
2. 创建 main.js 文件, 代码如下：

    	var fs = require("fs");

        	fs.readFile('input.txt', function (err, data) {
        	if (err) return console.error(err);
        	console.log(data.toString());
        	});

        	console.log("程序执行结束!");
    
3. 以上代码执行结果如下：

     	程序执行结束!
          	你好，我是于禤。
        	Hello!my name is yuxuan.
     
### 三.写文件（fs.writeFile()）
1. 创建一个空文件 output.txt

2. 创建 main.js 文件, 代码如下：

    	var fs = require('fs');

        	var data = 'Hello, Node.js';
        	fs.writeFile('output.txt', data, function (err) {
        	if (err) {
            	console.log(err);
        	} else {
            	console.log('ok.');
        	}
        	});

3. 执行结果：空文件 output.txt里的内容变为Hello, Node.js。
   writeFile()的参数依次为文件名、数据和回调函数。如果传入的数据是String，默认按UTF-8编码写入文本文件，如果传入的参数是Buffer，则写入的是二进制文件。回调函数由于只关心成功与否，因此只需要一个err参数。

4. 和readFile()类似，writeFile()也有一个同步方法，叫writeFileSync()：

    	var fs = require('fs');
        	var data = 'Hello, Node.js';
        	fs.writeFileSync('output.txt', data);

### 四.获取文件的详细信息（fs.stat()）
1. 创建一个文件 input.txt ，内容如下：

    	你好，我是于禤。
        	Hello!my name is yuxuan.

2. 创建 main.js 文件, 代码如下：

    	var fs = require('fs');

        	fs.stat('input.txt', function (err, stat) {
        	if (err) {
        	console.log(err);
        	} else {
        	// 是否是文件:
        	console.log('isFile: ' + stat.isFile());
        	// 是否是目录:
        	console.log('isDirectory: ' + stat.isDirectory());
        	if (stat.isFile()) {
            	// 文件大小:
            	console.log('size: ' + stat.size);
            	// 创建时间, Date对象:
            	console.log('birth time: ' + stat.birthtime);
            	// 修改时间, Date对象:
            	console.log('modified time: ' + stat.mtime);
       	 	}
     	}
        	});
    
3. 运行结果如下：
  
    	isFile: true
        	isDirectory: false
        	size: 51
        	birth time: Thu Oct 20 2016 13:40:21 GMT+0800 (中国标准时间)
        	modified time: Thu Oct 20 2016 14:04:41 GMT+0800 (中国标准时间)

