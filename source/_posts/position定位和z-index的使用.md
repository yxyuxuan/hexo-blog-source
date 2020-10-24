---
title: position定位和z-index的使用
date: 2016-07-26 23:49:24
tags: CSS
categories: CSS
---



## 浅说position定位及z-index使用

### 使用前提
- z-index只能在position属性值为relative或absolute或fixed的元素上有效。

### 基本原理
-  z-index值可以控制定位元素在垂直于显示屏方向（Z 轴）上的堆叠顺序（stack order），值大的元素发生重叠时会在值小的元素上面。

### 使用的相对性

- z-index值只决定同一父元素中的同级子元素的堆叠顺序。父元素的z-index值（如果有）为子元素定义了堆叠顺序（css版堆叠“拼爹”）。向上追溯找不到含有z-index值的父元素的情况下，则可以视为自由的z-index元素，它可以与父元素的同级兄弟定位元素或其他自由的定位元素来比较z-index的值，决定其堆叠顺序。同级元素的z-index值如果相同，则堆叠顺序由元素在文档中的先后位置决定，后出现的会在上面。

- 如果当你发现一个z-index值较大的元素被值较小的元素遮挡了，请先检查它们之间的dom结点关系，多半是因为其父结点含有激活并设置了z-index值的position定位元素。

- 因为这个相对性，会引发浏览器表现不一致出现兼容问题。原因是ie6、7下面position值为非static的元素在未设置z-index值的情况下都会被隐含添加z-index:0，而Firefox/Chrome等现代浏览器会遵循标准默认z-index:auto不会产生值。

- 还有一点需要注意，负值的z-index也依照大小比较的原理，但一般来说负值的z-index会被透明的body覆盖导致点击等事件响应出现问题，请谨慎使用。
## 元素位置重叠的背景常识

html文档中的元素默认处于普通流（normal flow）中，也就是说其顺序由元素在文档中的先后位置决定，此时一般不会产生重叠（但指定负边距可能产生重叠）。当我们用css为某个元素指定float浮动或者position定位后，元素的定位将会依情况发生如下改变：

 

1.  指定float值left/right

    行内元素也会隐形变成块元素，元素会脱离文档的普通流，向左或右浮动，直到其外边缘碰到包含框或另一个浮动框。 
2.  指定position值relative

    可以相对于其在普通流中的位置偏移，原本所占的空间仍保留。

3. 指定position值absolute

    行内元素也会隐形变成块元素，元素会脱离文档的普通流，相对于最近的已定位祖先元素偏移，如果元素没有已定位的祖先元素，那么它的位置相对于最初的包含块偏移。

4. 指定position值fixed

    元素会脱离文档的普通流，相对于浏览器窗口偏移，固定在浏览器的某个位置。

以上四种情况下，文档中的元素都将可能被浮动/定位元素覆盖产生重叠。

 

## 元素位置重叠的可能原因

1. 负边距/float浮动

    margin为负值时元素会依参考线向外偏移。margin-left/margin-top的参考线为左边的元素/上面的元素（如无兄弟元素则为父元素的左内侧/上内侧）,margin-right和margin-bottom的参考线为元素本身的border右侧/border下侧。一般可以利用负边距来就行布局，但没有计算好的话就可能造成元素重叠。堆叠顺序由元素在文档中的先后位置决定，后出现的会在上面。浮动元素会脱离文档的普通流，有可能覆盖或遮挡掉文档中的元素。

2. position的relative/absolute/fixed定位

    当为元素设置position值为relative/absolute/fixed后，元素发生的偏移可能产生重叠，且z-index属性被激活。z-index值可以控制定位元素在垂直于显示屏方向（Z 轴）上的堆叠顺序（stack order），值大的元素发生重叠时会在值小的元素上面。

3. window窗口元素引发的重叠

    浏览器解析页面时，先判断元素的类型：窗口元素优于非窗口元素显示（也就是窗口元素会覆盖在其它非窗口元素之上），同为非窗口类型才能在激活z-index属性控制堆叠顺序。

4. Flash元素属于window窗口元素

    所以如果页面上flash元素和其他元素发生重叠，需要先将flash嵌入的wmode属性的window（窗口，默认的会造成上面所说的问题）改为非窗口模式：opaque（非窗口不透明）或者 transparent（非窗口透明）。

5. ie6下select属于window类型控件

    同理，它也产生窗口元素的遮挡问题。解决方法使用iframe（原理：ie6下普通元素无法覆盖select，iframe可以覆盖select，普通元素可以覆盖iframe）/用div模拟实现select的效果。我一般会为被select遮挡的div在内部追加（appendChild）一个空的子iframe，设置position:absolute脱离文档流空间、width:100%;height:100%;覆盖整个父div、z-index:-1;确保值要小于父div的z-index值让父div覆盖显示在iframe上面，借助这个iframe来覆盖select。

 

