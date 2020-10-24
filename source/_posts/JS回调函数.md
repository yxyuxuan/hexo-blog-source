---
title: JS回调函数
date: 2017-01-25 19:04:55
tags: JavaScript
categories: JavaScript
---

# js回调函数的理解

**在认识js回调函数之前，首先要了解“函数也是一种数据类型”，它也可以像变量一样使用**

    1、var a = function(){console.log("function a");}
    
    2、function a(){console.log("function a");}

以上两种函数定义方式最终效果是一样的

举个例子：

    function a(){console.log("function a");}
    
    var b = a;//此时将function a赋值给b
    
    b();//执行b函数输出“function a”
    
    delete a;//删除a
    
    a();//undefined

**回调函数：**简单通俗点就是当有a和b两个函数，当a作为参数传给b，并在b中执行，这时a就是一个回调(callback)函数,如果a是一个匿名函数，则为匿名回调函数

在使用jquery时，经常会使用到回调，例如：

**1、绑定事件**

    $("p").blur( function () { alert("Hello World!"); } );
    
    $("p").click( function () { $(this).hide(); });

**2、效果动作**

    $("p").show("fast",function(){
    $(this).text("Animation Done!");
    });

**3、ajax操作**

    $("#feeds").load("feeds.php", {limit: 25}, function(){
    alert("The last 25 entries in the feed have been loaded");
    });



**如何自定义一个回调函数呢？**

举个小例子：

想要执行一个加法运算操作，在加操作执行完毕，我们还想要将这个加的结果作一个2倍操作，并返回最终值

    function double(data){
        return data*2;
    }
    
    function add(a,b,double){
        var sum = a+b; return double(sum);
    }

从上面代码可知，double函数作为参数传给add函数，add函数中执行完sum=a+b;后执行了double函数，double函数就是回调函数。
    
我对回调函数的理解是，回调就是一个函数的调用过程。函数a有一个参数，这个参数是个函数b，当函数a执行完以后执行函数b。函数b就是回调函数。那么这个过程就叫回调。