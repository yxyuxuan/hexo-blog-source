---
title: 用webpack2.6.1构建vue2.3.4单文件组件实例
date: 2017-03-30 19:11:27
tags: webpack，vue.js
categories: 前端
---


**1.初始化项目**

首先新建一个vue-webpack文件夹，在该文件夹路径下进行初始化项目。初始化项目后将自动生成package.json文件（这是一个标准的npm说明文件，里面蕴含了丰富的信息，包括当前项目的依赖模块，自定义的脚本任务等。）命令如下：

    npm init -y 

**2.安装各种依赖项**

a: 安装vue2.3.4

    npm install --save vue

b: 安装最新版本的webpack以及webpack测试服务器

    npm install --save-dev webpack webpack-dev-server

c: 安装babel，一般的浏览器是不认识es6语法的，babel的作用是将es6的语法编译成浏览器认识的语法

    npm install --save-dev babel-core babel-loader babel-preset-es2015

d: 安装vue-loader用来解析vue的组件，.vue后缀的文件。

    npm install --save-dev vue-loader vue-template-compiler

e: 安装css-loader file-loader解析CSS.

    npm install --save-dev css-loader file-loader 

**3. 编写页面**

在vue-webpack文件夹新建目录src，src目录下新建App.vue文件，代码如下：

    <template>
        <div id="example">
            <h1>{{ message }}</h1>
        </div>
    </template>
    
    <script>
        export default {
            data () {
                return {
                    message: 'Hello World !'
                }
            }
        }
    </script>
    
    <style scoped>
        #example {
            background:#000000;
        　　color:#fff;
            height: 100vh;
        }
    </style>


在src文件夹里新建main.js文件夹，代码如下：


    /* 引入vue和主页 */
    import Vue from 'vue'
    import App from './App.vue'
    
    /* 实例化一个vue */
    new Vue({
      el: '#app',
      render: h => h(App)
    })


**配置webpack**

在vue-webpack文件夹下新建webpack.config.js文件，代码如下：

    /* 引入操作路径模块和webpack */
    var path = require('path');
    var webpack = require('webpack');
    
    module.exports = {
        /* 输入文件 */
        entry: './src/main.js',
        output: {
            /* 输出目录，没有则新建 */
            path: path.resolve(__dirname, './dist'),
            /* 静态目录，可以直接从这里取文件 */
            publicPath: '/dist/',
            /* 文件名 */
            filename: 'build.js'
        },
        module: {
            rules: [
                /* 用来解析vue后缀的文件 */
                {
                    test: /\.vue$/,
                    loader: 'vue-loader'
                },
                /* 用babel来解析js文件并把es6的语法转换成浏览器认识的语法 */
                {
                    test: /\.js$/,
                    loader: 'babel-loader',
                    /* 排除模块安装目录的文件 */
                    exclude: /node_modules/
                }
            ]
        }
    }


**打包项目**

全局安装webpack

    npm install -g webpack

用webpack命令进行打包项目

    webpack

打包后在vue-webpack文件夹下会多出一个dist文件夹，查看里面会有build.js文件，里面的es6语法已经被转化了。



在根目录下新建index.html，引入build.js

    <!doctype html>
    <html>
    <head>
    <meta charset="utf-8">
    <title>vue-webpack</title>
    </head>
    <body>
        <section id="app"></section>
        <script src="./dist/build.js"></script>
    </body>
    </html>

**项目目录结构**

![](https://yxyuxuan.github.io/WeiXinErrorImages/vue-webpack1.png)

**热重载**

由于每修改一次都要重新打包，于是需要线上的热重载，全局安装webpack-dev-server：

    npm install -g webpack-dev-server 

使用webpack-dev-server命令运行：

    webpack-dev-server 

等待程序运行完毕，在浏览器输入http://localhost:8080/查看页面如下：

![](https://yxyuxuan.github.io/WeiXinErrorImages/vue-webpack.png)


