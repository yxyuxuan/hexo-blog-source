---
title: Node.js中的module.exports与exports的区别
date: 2016-10-31 20:34:52
tags: Node.js
categories: 前端
---

> 每一个node.js执行文件，都自动创建一个module对象，同时，module对象会创建一个叫exports的属性，初始化的值是 {}

     module.exports = {};

Node.js为了方便地导出功能函数，node.js会自动地实现以下这个语句

    //foo.js
    
         exports.a = function(){
             console.log('a')
         }
         exports.a = 1 


    //test.js
    
         var x = require('./foo');
         console.log(x.a)


   result:
           
    1    

这里，exports是引用 module.exports的值。

> module.exports 被改变的时候，exports不会被改变，而模块导出的时候，真正导出的执行是module.exports，而不是exports
再看看下面例子

    //foo.js
    
         exports.a = function(){
            console.log('a')
         }
     module.exports = {a: 2}
     exports.a = 1 


    //test.js
    
     var x = require('./foo');
     console.log(x.a)


result:2

exports在module.exports 被改变后，失效。


> 是不是开始有点廓然开朗，下面将会列出开源模块中，经常看到的几个使用方式。

### 1. module.exports = View

    function View(name, options) { 
       options = options || {};
       this.name = name;
       this.root = options.root;
       var engines = options.engines;
       this.defaultEngine = options.defaultEngine;
       var ext = this.ext = extname(name);
       if (!ext && !this.defaultEngine) throw new Error('No default engine was specified and no         extension was provided.');
       if (!ext) name += (ext = this.ext = ('.' != this.defaultEngine[0] ? '.' : '') +     this.defaultEngine);
       this.engine = engines[ext] || (engines[ext] = require(ext.slice(1)).__express);
       this.path = this.lookup(name);
     }
    
     module.exports = View;

javascript里面有一句话，函数即对象，View 是对象，module.export =View, 即相当于导出整个view对象。外面模块调用它的时候，能够调用View的所有方法。不过需要注意，只有是View的静态方法的时候，才能够被调用，prototype创建的方法，则属于View的私有方法。

    //foo.js
    
         function View(){
    
         }
    
         View.prototype.test = function(){
          console.log('test')
         }
    
         View.test1 = function(){
              console.log('test1')
         }
         module.exports = View


    //test.js
    
         var x = require('./foo');
    
         console.log(x) //{ [Function: View] test1: [Function] }
         console.log(x.test) //undefined
         console.log(x.test1) //[Function]
         x.test1() //test1

### 2. var app = exports = module.exports = {};

其实，当我们了解到原理后，不难明白这样的写法有点冗余，其实是为了保证，模块的初始化环境是干净的。同时也方便我们，即使改变了 module.exports 指向的对象后，依然能沿用 exports的特性

     exports = module.exports = createApplication;
    
     /**
      * Expose mime.
      */
    
     exports.mime = connect.mime;

例子当中module.exports = createApplication改变了module.exports了，让exports失效，通过exports = module.exports的方法，让其恢复原来的特点。

### 3. exports.init= function(){}

这种最简单，直接就是导出模块 init的方法。

### 4.  var mongoose = module.exports = exports = new Mongoose;

集多功能一身。