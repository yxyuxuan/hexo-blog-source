---
title: Backbone框架的小结
date: 2021-02-21 19:54:45
tags: Backbone
categories: 前端
---

> 维护一个使用Backbone框架的前端项目近三个月了，也是因为这个项目开始了学习和研究Backbone。
>
> Backbone诞生于2010年，目前看该框架有些old。Backbone更关注**组件化**和**作用域控制**等方面，从前端技术发展趋势的角度而言，目前层出不穷的现代化前端框架的诞生，都可以认为是Angular和Backbone等古典前端框架设计思想走向融合之后的产物。

####  Backbone介绍：

Backbone.js为复杂WEB应用程序提供模型(**model**)、集合(**collection**)、视图(**view**)的结构。其中模型用于绑定键值数据和自定义事件；集合附有可枚举函数的丰富API； 视图可以声明事件处理函数，并通过RESRful JSON接口连接到应用程序。

通过Backbone，可以将数据呈现为 [Models](https://www.backbonejs.com.cn/#Model), 你可以对模型进行创建，验证和销毁，以及将它保存到服务器。 任何时候只要UI事件引起模型内的属性变化，模型会触发*"change"*事件； 所有显示模型数据的 [Views](https://www.backbonejs.com.cn/#View) 会接收到该事件的通知，继而视图重新渲染。 你无需查找DOM来搜索指定*id*的元素去手动更新HTML。 — 当模型改变了，视图便会自动变化。

Backbone依赖[Underscore.js](https://underscorejs.net/)和jQuery。

Backbone文档：https://www.backbonejs.com.cn/

#### Backbone实践：

1. 视图(**View**)

   Backbone 视图几乎约定比他们的代码多 — 他们并不限定你的HTML或CSS， 并可以配合使用任何JavaScript模板库( [Mustache.js](http://github.com/janl/mustache.js), [Haml-js](http://github.com/creationix/haml-js)等)。通过绑定视图的 `render` 函数到模型的 `"change"` 事件 — 模型数据会即时的显示在 UI 中。

   // test_view.js

   ```javascript
   var TestModel = require('test_model.js');
   var TestView = Backbone.View.extend({
       tagname:'div',
       className: "test_content",
       template: _.template($('#template_test').html()),
       events: {
         "click .view_more": "viewMore",
     	},
       initialize: function(options) {
         this.model = new TestModel();
         this.listenTo(this.model, 'change', this.render);
       },
       render: function() {
         var data = this.model.toJSON();
         this.$el.html(this.template(data));
       }
       viewMore: function() {
      	  this.model.set({showMore:true});
   	}
   })
   module.exports = TestView;
   ```

   // test_template.haml

   ```html
   %script#template_test type="text/template"
     %div.test_box
     	%h3
     	  <%=title%>
     	%div
     	  <%=content%>
     	%a.view_more
     	  点击查看更多内容
   ```

2. 模型(**Models**)

   **Models（模型）**是任何Javascript应用的核心，包括数据交互及与其相关的大量逻辑： 转换、验证、计算属性和访问控制。

   // test_model.js

   ```javascript
   var TestModel = Backbone.Model.extend({
       idAttribute:'test_id',
       defaults: {
           title: '标题',
       	content: '内容',
       	showMore: false
       }
   })
   module.exports = TestModel;
   ```

3. 集合(**Collection**)

   集合是模型的有序组合，可以在集合上绑定 `"change"` 事件，从而当集合中的模型发生变化时`fetch`（获得）通知，集合也可以监听 `"add"` 和 `"remove"` 事件， 从服务器更新，并能使用 [Underscore.js](https://www.backbonejs.com.cn/#Collection-Underscore-Methods) 提供的方法。

   // test_collection.js

   ```javascript
   var TestModel = require('test_model.js');
   var TestCollection = Backbone.Collection.extend({
     model: new TestModel()
   });
   var TestGroup = new TestCollection([
       {
         title: "标题1",
         content: "内容1",
         showMore: false
       },
       {
         title: "标题2",
         content: "内容2",
         showMore: false
       }
   ])
   module.exports = TestGroup;
   ```

   

