---
title: Vue.js中的表单控件绑定
date: 2016-10-28 20:28:57
tags: Vue.js
categories: 前端
---


## 基础用法

可以用 v-model 指令在表单控件元素上创建双向数据绑定。根据控件类型它自动选取正确的方法更新元素。尽管有点神奇，v-model 不过是语法糖，在用户输入事件中更新数据，以及特别处理一些极端例子。

1.text

    <span>Message is: {{ message }}</span>
    <br>
    <input type="text" v-model="message" placeholder="edit me">

<br>  
result:

Message is:

 <input type="text" v-model="message" placeholder="edit me">

2.multiline text

    <span>Multiline message is:</span>
    <p>{{ message }}</p>
    <br>
    <textarea v-model="message" placeholder="add multiple lines"></textarea>

result:

Message is:

 <textarea v-model="message" placeholder="add multiple lines"></textarea>

3.Checkbox

单个勾选框，逻辑值：

    <input type="checkbox" id="checkbox" v-model="checked">
    <label for="checkbox">{{ checked }}</label>

 <input type="checkbox" id="checkbox" v-model="checked">false

4.多个勾选框，绑定到同一个数组：

    //.html
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
    <label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
    <br>
    <span>Checked names: {{ checkedNames | json }}</span>


    //.js
    new Vue({
      el: '...',
      data: {
        checkedNames: []
      }
    })

5.Radio

    <input type="radio" id="one" value="One" v-model="picked">
    <label for="one">One</label>
    <br>
    <input type="radio" id="two" value="Two" v-model="picked">
    <label for="two">Two</label>
    <br>
    <span>Picked: {{ picked }}</span>


 <input type="radio" id="one" value="One" v-model="picked">
    <label for="one">One</label>
    <br>
    <input type="radio" id="two" value="Two" v-model="picked">
    <label for="two">Two</label>
    <br>
    <span>Picked: 
    
6.Select
单选：

    <select v-model="selected">
      <option selected>A</option>
      <option>B</option>
      <option>C</option>
    </select>
    <span>Selected: {{ selected }}</span>

<select v-model="selected">
  <option selected>A</option>
  <option>B</option>
  <option>C</option>
</select>
<br>
<span>Selected:["A"]</span>

7.绑定 value

对于单选按钮，勾选框及选择框选项，v-model 绑定的 value 通常是静态字符串（对于勾选框是逻辑值）：

    <!-- 当选中时，`picked` 为字符串 "a" -->
    <input type="radio" v-model="picked" value="a">
    
    <!-- `toggle` 为 true 或 false -->
    <input type="checkbox" v-model="toggle">
    
    <!-- 当选中时，`selected` 为字符串 "abc" -->
    <select v-model="selected">
      <option value="abc">ABC</option>
    </select>

但是有时我们想绑定 value 到 Vue 实例的一个动态属性上，这时可以用 v-bind 实现，并且这个属性的值可以不是字符串。

​    
