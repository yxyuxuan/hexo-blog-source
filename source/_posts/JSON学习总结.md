---
title: JSON学习总结
date: 2017-02-20 15:13:26
tags: JavaScript
categories: 前端
---

## 一. JSON介绍
1. JSON 类似 XML
    - JSON 是纯文本
    - JSON 具有“自我描述性”（人类可读）
    - JSON 具有层级结构（值中存在值）
    - JSON 可通过 JavaScript 进行解析
    - JSON 数据可使用 AJAX 进行传输
    
2. 相比 XML 的不同之处
    - 没有结束标签
    - 更短
    - 读写的速度更快
    - 能够使用内建的 JavaScript eval() 方法进行解析
    - 使用数组
    - 不使用保留字
3. 为什么使用 JSON？
   	对于 AJAX 应用程序来说，JSON 比 XML 更快更易使用：
    - 使用 XML
        - 读取 XML 文档
        - 使用 XML DOM 来循环遍历文档
        - 读取值并存储在变量中
    - 使用 JSON
        - 读取 JSON 字符串
        - 用 eval() 处理 JSON 字符串
4. JSON 实例

   	通过我们的编辑器，您可以在线编辑 JavaScript 代码，然后通过点击一个按钮来查看结果：
    
        <html>
        <body>
        <h2>在 JavaScript 中创建 JSON 对象</h2>
        
        <p>
        Name: <span id="jname"></span><br />
        Age: <span id="jage"></span><br />
        Address: <span id="jstreet"></span><br />
        Phone: <span id="jphone"></span><br />
        </p>
        
        <script type="text/javascript">
        var JSONObject= {
        "name":"Bill Gates",
        "street":"Fifth Avenue New York 666",
        "age":56,
        "phone":"555 1234567"};
        document.getElementById("jname").innerHTML=JSONObject.name
        document.getElementById("jage").innerHTML=JSONObject.age
        document.getElementById("jstreet").innerHTML=JSONObject.street
        document.getElementById("jphone").innerHTML=JSONObject.phone
        </script>
        
        </body>
        </html>

## 二. JSON语法规则
1. JSON 语法是 JavaScript 对象表示法语法的子集。
    - 数据在名称/值对中
    - 数据由逗号分隔
    - 花括号保存对象
    - 方括号保存数组

2. JSON 名称/值对

    JSON 数据的书写格式是：名称/值对。
    名称/值对包括字段名称（在双引号中），后面写一个冒号，然后是值：
    
        "firstName" : "John"
    这很容易理解，等价于这条 JavaScript 语句：
    
        firstName = "John"
    
3. JSON 值
  
    JSON 值可以是：
    - 数字（整数或浮点数）
    - 字符串（在双引号中）
    - 逻辑值（true 或 false）
    - 数组（在方括号中）
    - 对象（在花括号中）
    - null

4. JSON 对像

       { "firstName":"John" , "lastName":"Doe" }

    JSON 对象在花括号中书写，对象可以包含多个名称/值对，与这条 JavaScript 语句等价：

        firstName = "John"
        lastName = "Doe"
  
5. JSON 数组

    JSON 数组在方括号中书写：
    数组可包含多个对象：
    
        {
        "employees": [
        { "firstName":"John" , "lastName":"Doe" },
        { "firstName":"Anna" , "lastName":"Smith" },
        { "firstName":"Peter" , "lastName":"Jones" }
        ]
        }
    在上面的例子中，对象 "employees" 是包含三个对象的数组。每个对象代表一条关于某人（有姓和名）的记录。
    
6. JSON 使用 JavaScript 语法
  
    因为 JSON 使用 JavaScript 语法，所以无需额外的软件就能处理 JavaScript 中的 JSON。
    通过 JavaScript，您可以创建一个对象数组，并像这样进行赋值：

        var employees = [
        { "firstName":"Bill" , "lastName":"Gates" },
        { "firstName":"George" , "lastName":"Bush" },
        { "firstName":"Thomas" , "lastName": "Carter" }
        ];
    
    可以像这样访问 JavaScript 对象数组中的第一项：
    
        employees[0].lastName;
    返回的内容是：
    
        Gates
    可以像这样修改数据：
    
        employees[0].lastName = "Jobs";

## 二.JSON 解析与序列化

1. JSON 最常见的用法之一，是从 web 服务器上读取 JSON 数据（作为文件或作为 HttpRequest），将 JSON 数据转换为 JavaScript 对象，然后在网页中使用该数据。
  
2. 由于 JSON 语法是 JavaScript 语法的子集，在旧版浏览器中，使用eval()对JSON数据结构求职存在危险，因为可能会执行一些恶意代码。
3. JSON对象有两个方法：stringify()和parse().在最简单的情况下，这两个方法分别用于把javascript对象序列化为JSON字符串和把JSON字符串解析为原生javascript值。

    **把javascript对象序列化为JSON字符串**
  
        <script type="text/javascript">
            var book = {
                            title: "Professional JavaScript",
                            authors: [
                                "Nicholas C. Zakas"
                            ],
                            edition: 3,
                            year: 2011
                       };
        
            var jsonText = JSON.stringify(book, ["title", "edition"]);
            alert(jsonText);
    
        </script>
    
    **把 JSON 字符串转换为 JavaScript 对象**
    
        <script type="text/javascript">
        var book = {
                       "title": "Professional JavaScript",
                        "authors": [
                            "Nicholas C. Zakas"
                        ],
                        edition: 3,
                        year: 2011,
                        releaseDate: new Date(2011, 11, 1)
                   };
    
        var jsonText = JSON.stringify(book);
        alert(jsonText);
        
        var bookCopy = JSON.parse(jsonText, function(key, value){
            if (key == "releaseDate"){
                return new Date(value);
            } else {
                return value;
            }
        });
        
        alert(bookCopy.releaseDate.toISOString());

    </script>