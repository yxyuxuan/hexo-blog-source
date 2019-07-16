---
title: Vue.js中的条件渲染和列表渲染
date: 2016-10-28 20:25:41
tags: Vue.js
categories: 前端
---


## 一.条件渲染
### 1.v-if

例子：

    //.html
    <div id="vm">  
        <h1 v-if="ok">ok</h1>
        <h1 v-else>no</h1>
    </div>



    //.js
    window.onload=function(){
      var vm = new Vue({
        el:'#vm',
        data:{
            ok:true,
            no:false
            }
        });
    }

result:

    ok

### 2.template v-if

因为 v-if 是一个指令，需要将它添加到一个元素上。但是如果我们想切换多个元素呢？此时我们可以把一个 <template> 元素当做包装元素，并在上面使用 v-if，最终的渲染结果不会包含它。

例子：

    <template v-if="ok">
      <h1>Title</h1>
      <p>Paragraph 1</p>
      <p>Paragraph 2</p>
    </template>

### 3.v-show

v-show指令的用法和v-if用法大体上一样

例子：

    <h1 v-show="ok">Hello!</h1>

**注意：** 不同的是有 v-show 的元素会始终渲染并保持在 DOM 中。v-show 是简单的切换元素的 CSS 属性 display。

注意 v-show 不支持 <template> 语法。

### 4.v-else

可以用 v-else 指令给 v-if 或 v-show 添加一个 “else 块”：

    <div v-if="Math.random() > 0.5">
          Sorry
    </div>
    <div v-else>
          Not sorry
    </div>

**注意：** v-else 元素必须立即跟在 v-if 或 v-show 元素的后面——否则它不能被识别。

## 二.列表渲染

### 1.v-for

a. 可以使用 v-for 指令基于一个数组渲染一个列表。这个指令使用特殊的语法，形式为 item in items，items 是数据数组，item 是当前数组元素的别名：

例：

    //.html
    <ul id="example-1">
      <li v-for="item in items">
        {{ item.message }}
      </li>
    </ul>



    //.js
    var example1 = new Vue({
      el: '#example-1',
      data: {
        items: [
          { message: 'Foo' },
          { message: 'Bar' }
        ]
      }
    })

result:

- Foo
- Bar


b. 也可以使用 v-for 遍历对象。除了 $index 之外，作用域内还可以访问另外一个特殊变量 $key。

例子：

    //.html
    <ul id="repeat-object" class="demo">
      <li v-for="value in object">
        {{ $key }} : {{ value }}
      </li>
    </ul>



    //.js
    new Vue({
      el: '#repeat-object',
      data: {
        object: {
          FirstName: 'John',
          LastName: 'Doe',
          Age: 30
        }
      }    
    })

result:

- FirstName:John
- lastname:Doe
- Age:30

**注意：** 在遍历对象时，是按 Object.keys() 的结果遍历，但是不能保证它的结果在不同的 JavaScript 引擎下是一致的。

c. v-for也可以接收一个整数，此时它将重复模板数次。

    <div>
        <span v-for="n in 10">{{n}}</span>
    </div>

result:

    0123456789


​    
###  2.template v-for

类似于 template v-if，也可以将 v-for 用在 <template> 标签上，以渲染一个包含多个元素的块。例如：

    <ul>
      <template v-for="item in items">
        <li>{{ item.msg }}</li>
        <li class="divider"></li>
      </template>
    </ul>


​    