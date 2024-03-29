---
title: React和Vue的比较
date: 2020-05-08 12:08:49
tags: React
categories: React
---

> 在面对自学前端三大主流框架（React、Vue、Angular）选择时，相信不少人选择了Vue。
>
> 从事前端工作三年有余，大四实习的时候选择了Vue框架，仅仅是因为Vue学习成本较低和入门相对容易些。毕业后进入现在的公司用的是React，就开始了React的学习探索直到现在。

#### React和Vue的使用比较：

​	相比Vue来说，React提供的API的确较少，比如Vue中的v-if，v-for之类的指令在React中需要自行用js实现，这也让React能够更大程度的发挥js的灵活性，能更自由的组合js，可以用js的if实现v-if，js的for实现v-for。React的概念简洁（自由度高），Vue的使用便利（提供现成的范式），React相对Vue规矩的多，这是因为其目标并非写更少的代码，而是追求条理更好理解，这种极高的代码规范在大型项目上是非常可贵的，可以减少不稳定因素的影响，很适合团队开发。

​	React的一大优势是把用户界面抽象成一个个组件，如按钮组件Button，对话框组件Dialog，日期组件Calendar。开发者通过组合这些组件，最终得到功能丰富、可交互的页面。通过引入JSX语法，使得编写组件简单快速，同时保证组件结构清晰。

### React

React起源于Facebook的内部项目，用来架设Instagram网站（Instagram在中国大陆简称**ins**，是Facebook公司旗下一款免费提供在线图片及视频分享的社交网站），并于 2013年 5 月开源。

React是用于构建用户界面的JS框架。因此react只负责解决view层的渲染。

React拥有较高的性能，代码逻辑简单，越来越多的人开始关注和使用。

React做了什么？

1. Virtual Dom模型

2. 生命周期管理

3. setState机制

4. diff算法

5. React patch、事件系统

6. React的 Virtual Dom模型（virtual dom 实际上是对实际Dom的一个抽象，是一个js对象。react所有的表层操作实际上是在操作virtual dom。）

7. diff算法用于计算出两个virtual dom的差异，是react中开销最大的地方。

   传统diff算法通过循环递归对比差异，算法复杂度为O(n3)。

   react diff算法制定了三条策略，将算法复杂度从 O(n3)降低到O(n)。

   WebUI中DOM节点跨节点的操作特别少，可以忽略不计。
   拥有相同类的组件会拥有相似的DOM结构。拥有不同类的组件会生成不同的DOM结构。
   同一层级的子节点，可以根据唯一的ID来区分。
   针对这三个策略，react diff实施的具体策略是:

   **diff对树进行分层比较，只对比两棵树同级别的节点。**跨层级移动节点，将会导致节点删除，重新插入，无法复用。
   diff对组件进行类比较，类相同的递归diff子节点，不同的直接销毁重建。diff对同一层级的子节点进行处理时，会根据key进行简要的复用。两棵树中存在相同key的节点时，只会移动节点。
   另外，在对比同一层级的子节点时:

   diff算法会以新树的第一个子节点作为起点遍历新树，寻找旧树中与之相同的节点。

   如果节点存在，则移动位置。如果不存在，则新建一个节点。

   在这过程中，维护了一个字段lastIndex，这个字段表示已遍历的所有新树子节点在旧树中最大的index。
   在移动操作时，只有旧index小于lastIndex的才会移动。

   这个顺序优化方案实际上是基于一个假设，大部分的列表操作应该是保证列表基本有序的。
   可以推倒倒序的情况下，子节点列表diff的算法复杂度为O(n2)

特点：

1. 声明式设计：React采用声明范式，可以轻松描述应用。
2. 高效：React通过对DOM的模拟，最大限度的减少与DOM的交互。
3. 灵活：React可以与已知的库或框架（**个人觉得框架是要求你按照它提供的规则去写代码， 而库是多个工具函数的集合**。）很好的配合。

优点：

1. 速度快：在UI渲染过程中，React通过在虚拟DOM中的微操作来实现对实际DOM的局部更新。

2. 跨浏览器兼容：虚拟DOM帮助我们解决了跨浏览器问题，它为我们提供了标准化的API，甚至在IE8中都是没问题的。

3. 模块化：为你程序编写独立的模块化UI组件，这样当某个或某些组件出现问题是，可以方便地进行隔离。

4. 单向数据流：Flux是一个用于在JavaScript应用中创建单向数据层的架构，它随着React视图库的开发而被Facebook概念化。

5. 同构、纯粹的javascript：因为搜索引擎的爬虫程序依赖的是服务端响应而不是JavaScript的执行，预渲染你的应用有助于搜索引擎优化。

6. 兼容性好：比如使用RequireJS来加载和打包，而Browserify和Webpack适用于构建大型应用。它们使得那些艰难的任务不再让人望而生畏。

缺点：

React本身只是一个V（view）而已，并不是一个完整的框架，所以如果是大型项目想要一套完整的框架的话，基本都需要加上ReactRouter和Flux才能写大型应用。

#### React性能优化方案

由于react中性能主要耗费在于update阶段的diff算法，因此性能优化也主要针对diff算法。

1. 减少diff算法触发次数
   减少diff算法触发次数实际上就是减少update流程的次数。
   正常进入update流程有三种方式：
2. setState
   setState机制在正常运行时，由于批更新策略，已经降低了update过程的触发次数。
   因此，setState优化主要在于非批更新阶段中(timeout/Promise回调)，减少setState的触发次数。
   常见的业务场景即处理接口回调时，无论数据处理多么复杂，保证最后只调用一次setState。
3. 父组件render
   父组件的render必然会触发子组件进入update阶段（无论props是否更新）。此时最常用的优化方案即为shouldComponentUpdate方法。
   最常见的方式为进行this.props和this.state的浅比较来判断组件是否需要更新。或者直接使用PureComponent，原理一致。
   需要注意的是，父组件的render函数如果写的不规范，将会导致上述的策略失效。
4. 正确使用diff算法
   不使用跨层级移动节点的操作。
   对于条件渲染多个节点时，尽量采用隐藏等方式切换节点，而不是替换节点。
   尽量避免将后面的子节点移动到前面的操作，当节点数量较多时，会产生一定的性能问题。

### Vue

Vue是尤雨溪编写的一个构建数据驱动的Web界面的库，准确来说不是一个框架，它聚焦在V（view）视图层。

基本原理：

1. 建立虚拟DOM Tree，通过document.createDocumentFragment()，遍历指定根节点内部节点，根据{{ prop }}、v-model等规则进行compile；
2. 通过Object.defineProperty()进行数据变化拦截；
3. 截取到的数据变化，通过发布者-订阅者模式，触发Watcher，从而改变虚拟DOM中的具体数据；
4. 通过改变虚拟DOM元素值，从而改变最后渲染dom树的值，完成双向绑定。完成数据的双向绑定在于Object.defineProperty()

特点：

1. 轻量级的框架

2. 双向数据绑定

3. 指令

4. 插件化

优点：

1. 简单：官方文档很清晰，比Angular简单易学。

2. 快速：异步批处理方式更新DOM。

3. 组合：用解耦的、可复用的组件组合你的应用程序。

4. 紧凑：~18kbmin+gzip，且无依赖。

5. 强大：表达式无需声明依赖的可推导属性(computedproperties)。

6. 对模块友好：可以通过NPM、Bower或Duo安装，不强迫你所有的代码都遵循Angular的各种规定，使用场景更加灵活。

缺点：

1. 新生儿：Vue.js是一个新的项目，没有angular那么成熟。

2. 影响度不是很大：google了一下，有关于Vue.js多样性或者说丰富性少于其他一些有名的库

3. 不支持IE8。

### Angular

Angular是一款优秀的前端JS框架，已经被用于Google的多款产品当中。

特点：

1. 良好的应用程序结构

2. 双向数据绑定

3. 指令

4. HTML模板

5. 可嵌入、注入和测试

优点：

1. 模板功能强大丰富，自带了极其丰富的angular指令。

2. 是一个比较完善的前端框架，包含服务，模板，数据双向绑定，模块化，路由，过滤器，依赖注入等所有功能；3.自定义指令，自定义指令后可以在项目中多次使用。

4. ng模块化比较大胆的引入了Java的一些东西（依赖注入），能够很容易的写出可复用的代码，对于敏捷开发的团队来说非常有帮助。

5. angularjs是互联网巨人谷歌开发，这也意味着他有一个坚实的基础和社区支持。

缺点：

1. angular入门很容易但深入后概念很多,学习中较难理解。

2. 文档例子非常少,官方的文档基本只写了api,一个例子都没有,很多时候具体怎么用都是google来的,或直接问misko,angular的作者。

3. 对IE6/7兼容不算特别好,就是可以用jQuery自己手写代码解决一些。

4. 指令的应用的最佳实践教程少,angular其实很灵活,如果不看一些作者的使用原则,很容易写出四不像的代码,例如js中还是像jQuery的思想有很多dom操作。

5. DI依赖注入如果代码压缩需要显示声明。