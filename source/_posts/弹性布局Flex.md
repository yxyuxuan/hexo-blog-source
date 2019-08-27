---
title: 弹性布局Flex
date: 2019-08-27 15:20:23
tags: CSS
categories：前端
---

> 写了这末多项目了，每个项目的页面布局都离不开Flex布局，就连前段时间写的小程序项目都是用的Flex布局，下面简单整理一下Flex布局相关知识点。

Flex 是 Flexible Box 的缩写，Flex Box布局称为**Flex布局**或**弹性布局**。

**直接设置flex布局的元素称为flex容器，它里面的子元素称为flex项目。**

Flex容器默认有两根轴，主轴和交叉轴，交叉轴垂直于主轴。

### 一. 作用在flex容器上的相关属性

1. flex-direction

   `flex-direction`属性决定主轴的方向（即项目的排列方向）。

   ```css
   .box {
     flex-direction: row | row-reverse | column | column-reverse;
   }
   ```

   - `row`（默认值）：主轴为水平方向，起点在左端。

   - `row-reverse`：主轴为水平方向，起点在右端。

   - `column`：主轴为垂直方向，起点在上沿。

   - `column-reverse`：主轴为垂直方向，起点在下沿。

     

2. flex-wrap

   默认情况下，项目都排在一条线（又称"轴线"）上。`flex-wrap`属性定义，如果一条轴线排不下，如何换行。

   ```css
   .box{
     flex-wrap: nowrap | wrap | wrap-reverse;
   }
   ```

   - `nowrap`（默认）：不换行。

   - `wrap`：换行，第一行在上方。

   - `wrap-reverse`：换行，第一行在下方。

     

3. flex-flow

   `flex-flow`属性是`flex-direction`和`flex-wrap`的缩写，表示flex布局的flow流动特性，默认值为`row nowrap`。

   ```
   flex-flow: <‘flex-direction’> || <‘flex-wrap’>
   
   //例子：
   .container {
       display: flex;
       flex-flow: row-reverse wrap-reverse;
   }
   ```

4. justify-content 

   `justify-content`属性定义了项目在主轴上的对齐方式。

   ```css
   .box {
     justify-content: flex-start | flex-end | center | space-between | space-around;
   }
   ```

   - `flex-start`（默认值）：左对齐
   - `flex-end`：右对齐
   - `center`： 居中
   - `space-between`：两端对齐，项目之间的间隔都相等。
   - `space-around`：每个项目两侧的间隔相等。项目之间的间隔比项目与边框的间隔大一倍。

5. align-items

   `align-items`中的`items`指的就是flex子项目，因此`align-items`指的就是flex子项目相对于flex容器在垂直方向（交叉轴）上的对齐方式

   ```css
   .box {
     align-items: flex-start | flex-end | center | baseline | stretch;
   }
   ```

   - `flex-start`：交叉轴的起点对齐。
   - `flex-end`：交叉轴的终点对齐。
   - `center`：交叉轴的中点对齐。
   - `baseline`: 项目的第一行文字的基线对齐。
   - `stretch`（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。

6. align-content

   `align-content`可以看成和`justify-content`是相似且对立的属性，`justify-content`指明水平方向flex子项的对齐和分布方式，而`align-content`则是指明垂直方向每一行flex元素的对齐和分布方式。如果所有flex子项只有一行，则`align-content`属性是没有任何效果的

   ```css
   .box {
     align-content: flex-start | flex-end | center | space-between | space-around | stretch;
   }
   ```

   - `flex-start`：与交叉轴的起点对齐。
   - `flex-end`：与交叉轴的终点对齐。
   - `center`：与交叉轴的中点对齐。
   - `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布。
   - `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
   - `stretch`（默认值）：轴线占满整个交叉轴。

### 二. 作用在子项目上的相关属性

1. order

   `order`属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

   ```css
   .item {
     order: <integer>;
   }
   ```

2. flex-grow

   `flex-grow`属性定义项目的放大比例，默认为`0`，即如果存在剩余空间，也不放大。

   ```css
   .item {
     flex-grow: <number>; /* default 0 */
   }
   ```

3. flex-shrink

   `flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。负值对该属性无效。

   ```css
   .item {
     flex-shrink: <number>; /* default 1 */
   }
   ```

   如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。

   

4. flex-basis

   `flex-basis`属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为`auto`，即项目的本来大小。

   ```css
   .item {
     flex-basis: <length> | auto; /* default auto */
   }
   ```

   它可以设为跟`width`或`height`属性一样的值（比如350px），则项目将占据固定空间。

   

5. flex

   `flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`。后两个属性可选。

   ```css
   .item {
     flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
   }
   ```

   该属性有两个快捷值：`auto` (`1 1 auto`) 和 none (`0 0 auto`)。

   建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值。

   

6. align-self

   `align-self`属性允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性。默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`。

   ```css
   .item {
     align-self: auto | flex-start | flex-end | center | baseline | stretch;
   }
   ```

###### 注：

在Flex布局中，flex子元素的设置float，clear以及vertical-align属性都是没有用的。

Flexbox布局最适合小规模布局（一维布局），而Grid布局则适用于更大规模的布局（二维布局）。

