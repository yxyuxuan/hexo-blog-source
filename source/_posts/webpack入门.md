---
title: webpack入门
date: 2017-03-18 10:10:44
tags: webpack
categories: 前端
---

### 1.介绍webpack
webpack 是当下最热门的前端资源模块化管理和打包工具。它可以将许多松散的模块按照依赖和规则打包成符合生产环境部署的前端资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候在异步加载。通过loader的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、 AMD 模块、 ES6 模块、CSS、图片、 JSON、Coffeescript、 LESS 等。

WebPack可以看做是模块打包机：它做的事情是，分析你的项目结构，找到JavaScript模块以及其它的一些浏览器不能直接运行的拓展语言（Scss，TypeScript等），并将其打包为合适的格式以供浏览器使用。

### 2.安装webpack
安装之前需要安装node.js，因为node.js自带了npm(软件包管理器)，使用npm安装webpack。

    npm install webpack -g

### 3.创建项目

a:新建一个demo文件夹

例如：demo文件夹的路径为 D:\Webpack Project\demo

b:在路径为D:\Webpack Project\demo下，使用 **npm init** 来创建一个package.json文件。这是一个标准的npm说明文件，里面蕴含了丰富的信息，包括当前项目的依赖模块，自定义的脚本任务等。

c:把webpack安装到当前的项目依赖中，在路径为D:\Webpack Project\demo下，使用 **npm install webpack --save-dev** 安装webpack

d:在demo文件夹下新建index.html

    <html>  
        <head>  
            <meta charset="UTF-8">  
            <title>Document</title>  
        </head>  
        <body>  
            <script type="text/javascript" src="bundle.js"></script>  
        </body>  
    </html>  

e:在demo文件夹下新建一个main.js文件

    document.write('<h1>Hello World !</h1>');

f:在demo文件夹下新建一个webpack.config.js文件

    module.exports = {  
      entry: './main.js',         //唯一入口文件 
      output: {  
        filename: 'bundle.js'     //打包后输出文件的文件名 
      }  
    }; 

把main.js文件打包为bundle.js文件。其中bundle.js文件是Webpack生成出来的。

g:打包，使用命令 **webpack** 来打包，然后使用浏览器打开index.html文件就能看到运行的结果了。


**创建第二个 JS 文件**

新建一个main2.js文件，代码如下所示：

    document.write('<h1>Hello World</h1>');

然后修改mian.js文件，代码如下所示：

    document.write(require("./main1.js"));

使用命令 webpack 来重新打包看到的结果是一样的。

**loader**

Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。所以如果我们需要在应用中添加 css 文件，就需要使用到 css-loader 和 style-loader，他们做两件不同的事情，css-loader 会遍历 CSS 文件，然后找到 url() 表达式然后处理他们，style-loader 会把原来的 CSS 代码插入页面中的一个 style 标签中。

使用命令安装css-loader和style-loader

    cnpm install css-loader style-loader

执行以上命令后，会再当前目录生成 node_modules 目录，它就是 css-loader 和 style-loader 的安装目录。

接下来创建一个style.css文件，代码如下：
body {
    background: lightblue;
}

修改main.js文件

    require("!style/!css!./style.css");
    document.write(require("./main1.js"));

修改webpack.config.js文件

    module.exports = {  
      entry: './main.js',   
      output: {  
        filename: 'bundle.js'  
      }，
      module:{
    	rules:[
    	{test:/\.css$/,loader:'style-loader!css-loader'}
    	]
      }  
    }; 

重新使用 **webpack** 命令打包，结果和上面一样。

**热重载**

因为每次修改都需要重新打包浪费时间，所以需要线上的热重载。使用如下命令全局安装webpack-dev-server。

    npm install -g webpack-dev-server

安装完成后使用如下命令运行：

    webpack-dev-server

运行完毕，在浏览器中输入**http://localhost:8080/webpack-dev-server/**查看页面。
如果这时修改页面的的代码，不需要刷新浏览器就能自动更改。