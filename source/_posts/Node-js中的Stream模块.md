---
title: Node.js中的Stream模块
date: 2016-10-20 21:12:32
tags: Node.js
categories: 前端
---


**注意：**

1. stream是Node.js提供的又一个仅在服务区端可用的模块，目的是支持“流”这种数据结构。

2. 什么是流？流是一种抽象的数据结构。想象水流，当在水管中流动时，就可以从某个地方（例如自来水厂）源源不断地到达另一个地方（比如你家的洗手池）。我们也可以把数据看成是数据流，比如你敲键盘的时候，就可以把每个字符依次连起来，看成字符流。这个流是从键盘输入到应用程序，实际上它还对应着一个名字：标准输入流（stdin）。
3. Node.js，Stream 有四种流类型：

 * Readable - 可读操作。
 * Writable - 可写操作。
 * Duplex - 可读可写操作.
 * Transform - 操作被写入数据，然后读出结果。

4. 所有的 Stream 对象都是 EventEmitter 的实例。常用的事件有：

	* data - 当有数据可读时触发。
	* end - 没有更多的数据可读时触发。
	* error - 在接收和写入过程中发生错误时触发。
	* finish - 所有数据已被写入到底层系统时触发。

----------

### 一.从流中读取数据

1. 创建 input.txt 文件，内容如下：

    	hello world !
    
2. 创建 main.js 文件, 代码如下：

    	var fs = require("fs");
        	var data = '';

        	// 创建可读流
        	var readerStream = fs.createReadStream('input.txt');
    
        	// 设置编码为 utf8。
        	readerStream.setEncoding('UTF8');
    
        	// 处理流事件 --> data, end, and error
        	readerStream.on('data', function(chunk) {
      	 	data += chunk;
      		});

        	readerStream.on('end',function(){
       		console.log(data);
        	});

        	readerStream.on('error', function(err){
       		console.log(err.stack);
        	});

      	 	console.log("程序执行完毕");


3. 以上代码执行结果如下：

    	程序执行完毕
        	hello world !

### 二.写入流

1. 创建 main.js 文件, 代码如下：

    	var fs = require("fs");
        	var data = 'hello world !';

        	// 创建一个可以写入的流，写入到文件 output.txt 中
        	var writerStream = fs.createWriteStream('output.txt');
    
        	// 使用 utf8 编码写入数据
        	writerStream.write(data,'UTF8');
    
        	// 标记文件末尾
        	writerStream.end();
    
        	// 处理流事件 --> data, end, and error
        	writerStream.on('finish', function() {
        	console.log("写入完成。");
        	});

        	writerStream.on('error', function(err){
       		console.log(err.stack);
        	});

        		console.log("程序执行完毕");
    
2. 以上程序会将 data 变量的数据写入到 output.txt 文件中。代码执行结果如下：

   		$ node main.js 
        	程序执行完毕
        	写入完成。
3. 查看 output.txt 文件的内容：

    	$ cat output.txt 
        	hello world !

### 三.管道流

 管道提供了一个输出流到输入流的机制。通常我们用于从一个流中获取数据并将数据传递到另外一个流中。

 以下实例我们通过读取一个文件内容并将内容写入到另外一个文件中。


1. 设置 input.txt 文件内容如下：

    	Hello! my name is yuxuan.

2. 创建 main.js 文件, 代码如下：

    	var fs = require("fs");

        	// 创建一个可读流
        	var readerStream = fs.createReadStream('input.txt');
    
        	// 创建一个可写流
        	var writerStream = fs.createWriteStream('output.txt');
    
        	// 管道读写操作
        	// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
        	readerStream.pipe(writerStream);
     
        	console.log("程序执行完毕");
    
3. 代码执行结果如下：

    	$ node main.js 
        	程序执行完毕
    
4. 查看 output.txt 文件的内容：

    	$ cat output.txt 
        	Hello! my name is yuxuan.

### 四.链式流

- 链式是通过连接输出流到另外一个流并创建多个对个流操作链的机制。链式流一般用于管道操作。
接下来我们就是用管道和链式来压缩和解压文件。

**a. 压缩文件**

1. 创建 compress.js 文件, 代码如下：

    	var fs = require("fs");
        	var zlib = require('zlib');

        	// 压缩 input.txt 文件为 input.txt.gz
        	fs.createReadStream('input.txt')
        	.pipe(zlib.createGzip())
        	.pipe(fs.createWriteStream('input.txt.gz'));
        
        	console.log("文件压缩完成。");
    
2. 代码执行结果如下：

		$ node compress.js 
    	文件压缩完成。
   
3. 执行完以上操作后，我们可以看到当前目录下生成了 input.txt 的压缩文件 input.txt.gz。

**b. 解压该文件**

1.  创建 decompress.js 文件，代码如下：

    	var fs = require("fs");
        	var zlib = require('zlib');

      	 	// 解压 input.txt.gz 文件为 input.txt
        	fs.createReadStream('input.txt.gz')
      	.pipe(zlib.createGunzip())
      	.pipe(fs.createWriteStream('input.txt'));
  
        	console.log("文件解压完成。");
    
2. 代码执行结果如下：

		$ node decompress.js 
     	文件解压完成。
