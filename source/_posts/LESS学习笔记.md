---
title: LESS学习笔记
date: 2017-05-10 10:09:31
tags: less
categories: CSS
---


> Less 是一门 CSS 预处理语言，它使用类似CSS的语法，为CSS的赋予了动态语言的特性，如变量、继承、运算、函数等，更方便CSS的编写和维护。

> Less 可以在多种语言、环境中使用，包括浏览器端、桌面客户端、服务端。


### 1.语法特性

**变量:**

变量允许我们单独定义一系列通用的样式，然后在需要的时候去调用。所以在做全局样式调整的时候我们可能只需要修改几行代码就可以了。

less源码:

    @color:#999;
    
    #header{
        color:@color;
    }
    h2{
        color:@color     
    }

编译后的css :

    #header {
        color: #999;
    }
    h2 {
        color: #999;
    }

**混合（Mixins）**

混合可以将一个定义好的class A轻松的引入到另一个class B中，从而简单实现class B继承class A中的所有属性。我们还可以带参数地调用，就像使用函数一样。

LESS源码：

    .rounded-corners (@radius: 5px) {
        -webkit-border-radius: @radius;
        -moz-border-radius: @radius;
        -ms-border-radius: @radius;
        -o-border-radius: @radius;
        border-radius: @radius;
    }
    
    #header {
        .rounded-corners;
    }
    #footer {
        .rounded-corners(10px);
    }

编译后的CSS：

    #header {
        -webkit-border-radius: 5px;
        -moz-border-radius: 5px;
        -ms-border-radius: 5px;
        -o-border-radius: 5px;
        border-radius: 5px;
    }
    #footer {
        -webkit-border-radius: 10px;
        -moz-border-radius: 10px;
        -ms-border-radius: 10px;
        -o-border-radius: 10px;
        border-radius: 10px;
    }

**嵌套**

我们可以在一个选择器中嵌套另一个选择器来实现继承，这样很大程度减少了代码量，并且代码看起来更加的清晰。

LESS源码：

    #header {
        h1 {
            font-size: 26px;
            font-weight: bold;
        }
        p {
            font-size: 12px;
            a {
                text-decoration: none;
                &:hover {
                    border-width: 1px
                }
            }
        }
    }
编译后的CSS：

    #header h1 {
        font-size: 26px;
        font-weight: bold;
    }
    #header p {
        font-size: 12px;
    }
    #header p a {
        text-decoration: none;
    }
    #header p a:hover {
        border-width: 1px;
    }

**函数和运算**

运算提供了加，减，乘，除操作；我们可以做属性值和颜色的运算，这样就可以实现属性值之间的复杂关系。LESS中的函数一一映射了JavaScript代码，如果你愿意的话可以操作属性值。

LESS源码：

    @the-border: 1px;
    @base-color: #111;
    @red: #842210;
    
    #header {
        color: (@base-color * 3);
        border-left: @the-border;
        border-right: (@the-border * 2);
    }
    #footer {
        color: (@base-color + #003300);
        border-color: desaturate(@red, 10%);
    }
编译后的CSS：

    #header {
        color: #333;
        border-left: 1px;
        border-right: 2px;
    }
    #footer {
        color: #114411;
        border-color: #7d2717;
    }

### 2.less 的使用

less的使用是很容易的，首先，使用你最常使用的代码编辑器，按LESSCSS的语法规则写好.less文件，接下来，使用编译工具它编译成.css，最后再引入页面即可。

**GUI编译工具**

为方便起见，建议初学者使用GUI编译工具来编译.less文件，以下是一些可选GUI编译工具：

1. koala(Win/Mac/Linux)

    国人开发的LESSCSS/SASS编译工具。下载地址：http://koala-app.com/index-zh.html

2. Codekit(Mac)

    一款自动编译Less/Sass/Stylus/CoffeeScript/Jade/Haml的工具，含语法检查、图片优化、自动刷新等附加功能。下载地址http://incident57.com/codekit/

3. WinLess(Win)

    一款LESS编译软件。下载地址http://winless.org/

4. SimpleLess(Win/Mac/Linux)

    一款LESS编译软件。下载地址http://wearekiss.com/simpless

**Node.js库**

LESSCSS官方有一款基于Node.js的库，用于编译.less文件。

使用时，首先全局安装less（部分系统下可能需要在前面加上sudo切换为超级管理员权限）：

    npm install -g less

接下来就可以使用lessc来编译.less文件了：

    lessc example/example.less example/example.css

更多选项可以直接运行lessc查看说明。

**浏览器端使用**

LESSCSS也可以不经编译，直接在浏览器端使用。

使用方法：

1. 下载LESSCSS的.js文件，例如lesscss-1.4.0.min.js。在页面中引入.less文件

        <link rel="stylesheet/less" href="example.less" />

2. 需要注意rel属性的值是stylesheet/less，而不stylesheet。

3. 引入第1步下载的.js文件

        <script src="lesscss-1.4.0.min.js"></script>
   
    需要特别注意的是，由于浏览器端使用时是使用ajax来拉取.less文件，因此直接在本机文件系统打开（即地址是file://开头）或者是有跨域的情况下会拉取不到.less文件，导致样式无法生效。

    还有一种情况容易导致样式无法生效，就是部分服务器（以IIS居多）会对未知后缀的文件返回404，导致无法正常读取.less文件。解决方案是在服务器中为.less文件配置MIME值为text/css（具体方法请搜索）。或者还有一种更简单的方法，即是直接将.less文件改名为.css文件即可。



