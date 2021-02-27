---
title: AJAX
date: 2017-01-20 14:00:29
tags: AJAX
categories: 前端
---

## 一. AJAX简介
1. AJAX = 异步 JavaScript 和 XML。
2. AJAX 是一种用于创建快速动态网页的技术。
3. 通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
4. 传统的网页（不使用 AJAX）如果需要更新内容，必需重载整个网页面。
5. 有很多使用 AJAX 的应用程序案例：新浪微博、Google 地图、开心网等等。

##  二. 创建 XMLHttpRequest 对象
1. XMLHttpRequest 是 AJAX 的基础。
2. 所有现代浏览器均支持 XMLHttpRequest 对象（IE5 和 IE6 使用 ActiveXObject）。
3. XMLHttpRequest 用于在后台与服务器交换数据。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。
4. 所有现代浏览器（IE7+、Firefox、Chrome、Safari 以及 Opera）均内建 XMLHttpRequest 对象。


5. 创建 XMLHttpRequest 对象的语法：

        variable=new XMLHttpRequest();
   
    老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：

        variable=new ActiveXObject("Microsoft.XMLHTTP");
   
6. 为了应对所有的现代浏览器，包括 IE5 和 IE6，请检查浏览器是否支持 XMLHttpRequest 对象。如果支持，则创建 XMLHttpRequest 对象。如果不支持，则创建 ActiveXObject ：

        var xmlhttp;
        if (window.XMLHttpRequest)
        {
            //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
            xmlhttp=new XMLHttpRequest();
        }
        else
        {
            // IE6, IE5 浏览器执行代码
            xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
        }
   
## 三.请求
1. 向服务器发送请求请求:
    - XMLHttpRequest 对象用于和服务器交换数据。
    - 如需将请求发送到服务器，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法：

        xmlhttp.open("GET","ajax_info.txt",true);
        xmlhttp.send();

2. open(method,url,async):规定请求的类型、URL 以及是否异步处理请求。
    - method：请求的类型；GET 或 POST
    - url：文件在服务器上的位置
    - async：true（异步）或 false（同步）

3. send(string):将请求发送到服务器。

    - string：仅用于 POST 请求

4. GET 请求

    一个简单的 GET 请求：
        
        xmlhttp.open("GET","/try/ajax/demo_get.php",true);
        xmlhttp.send();
5. POST 请求

    一个简单 POST 请求：

        xmlhttp.open("POST","/try/ajax/demo_post.php",true);
        xmlhttp.send();


6. GET 还是 POST？
    与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。
    然而，在以下情况中，请使用 POST 请求： 
     - 无法使用缓存文件（更新服务器上的文件或数据库）  
     - 向服务器发送大量数据（POST 没有数据量限制）
     - 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

## 四.响应
1. 服务器响应

    如需获得来自服务器的响应，请使用 XMLHttpRequest 对象的responseText 或 responseXML 属性。
   - responseText：获得字符串形式的响应数据。
   - responseXML：获得 XML 形式的响应数据。

2. responseText 属性

    如果来自服务器的响应并非 XML，请使用 responseText 属性。
   - responseText 属性返回字符串形式的响应，因此您可以这样使用：

            document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
   
3. responseXML 属性

    如果来自服务器的响应是 XML，而且需要作为 XML 对象进行解析，请使用 responseXML 属性：
        
        请求 cd_catalog.xml 文件，并解析响应：
        xmlDoc=xmlhttp.responseXML;
        txt="";
        x=xmlDoc.getElementsByTagName("ARTIST");
        for (i=0;i<x.length;i++)
        {
            txt=txt + x[i].childNodes[0].nodeValue + "<br>";
        }
        document.getElementById("myDiv").innerHTML=txt;
    
## 五.onreadystatechange 事件
1. 当请求被发送到服务器时，我们需要执行一些基于响应的任务。
2. 每当 readyState 改变时，就会触发 onreadystatechange 事件。
3. readyState 属性存有 XMLHttpRequest 的状态信息。
4. 下面是 XMLHttpRequest 对象的三个重要的属性：
   - onreadystatechange: 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。
   - readyState:存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。0: 请求未初始化1: 服务器连接已建立2: 请求已接收3: 请求处理中4: 请求已完成，且响应已就绪.    
   - status:  200: "OK" 404: 未找到页面

    在 onreadystatechange 事件中，我们规定当服务器响应已做好被处理的准备时所执行的任务。
   
    当 readyState 等于 4 且状态为 200 时，表示响应已就绪：

        xmlhttp.onreadystatechange=function()
          {
          if (xmlhttp.readyState==4 && xmlhttp.status==200)
            {
            document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
            }
          }
   
    注意： onreadystatechange 事件被触发 5 次（0 - 4），对应着 readyState 的每个变化。