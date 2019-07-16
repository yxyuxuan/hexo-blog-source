---
title: CSS3中transition属性
date: 2017-08-02 10:28:00
tags: CSS3
categories: 前端
---
### 一.transition描述：
W3C标准中对css3的transition这是样描述的：“css的transition允许css的属性值在一定的时间区间内平滑地过渡。这种效果可以在鼠标单击、获得焦点、被点击或对元素任何改变中触发，并圆滑地以动画效果改变CSS的属性值。”

### 二.transition语法：

	transition：[ transition-property ] || [ transition-duration ] || [ transition-timing-function ] || [ transition-delay ]

### 三.子属性简介：

#### 1.transition-property:

transition-property是用来指定当元素其中一个属性改变时执行transition效果，其主要有以下几个值：none(没有属性改变)；all（所有属性改变）这个也是其默认值；indent（元素属性名）。当其值为none时，transition马上停止执行，当指定为all时，则元素产生任何属性值变化时都将执行transition效果，ident是可以指定元素的某一个属性值。其对应的类型如下:

1、color: 通过红、绿、蓝和透明度组件变换（每个数值处理）如：background-color,border-color,color,outline-color等css属性；

2、length: 真实的数字 如：word-spacing,width,vertical-align,top,right,bottom,left,padding,outline-width,margin,min-width,min-height,max-width,max-height,line-height,height,border-width,border-spacing,background-position等属性；

3、percentage:真实的数字 如：word-spacing,width,vertical-align,top,right,bottom,left,min-width,min-height,max-width,max-height,line-height,height,background-position等属性；

4、integer离散步骤（整个数字），在真实的数字空间，以及使用floor()转换为整数时发生 如：outline-offset,z-index等属性；

5、number真实的（浮点型）数值，如：zoom,opacity,font-weight,等属性；

6、transform list:详情请参阅：《CSS3 Transform》

7、rectangle:通过x, y, width 和 height（转为数值）变换，如：crop

8、visibility: 离散步骤，在0到1数字范围之内，0表示“隐藏”，1表示完全“显示”,如：visibility

9、shadow: 作用于color, x, y 和 blur（模糊）属性,如：text-shadow

10、gradient: 通过每次停止时的位置和颜色进行变化。它们必须有相同的类型（放射状的或是线性的）和相同的停止数值以便执行动画,如：background-image

11、paint server (SVG): 只支持下面的情况：从gradient到gradient以及color到color，然后工作与上面类似

12、space-separated list of above:如果列表有相同的项目数值，则列表每一项按照上面的规则进行变化，否则无变化

13、a shorthand property: 如果缩写的所有部分都可以实现动画，则会像所有单个属性变化一样变化

#### 2.transition-duration:

transition-duration是用来指定元素 转换过程的持续时间，取值：<time>为数值，单位为s（秒）或者ms(毫秒),可以作用于所有元素，包括:before和:after伪元素。其默认值是0，也就是变换时是即时的。

#### 3.transition-timing-function:

transition-timing-function的值允许你根据时间的推进去改变属性值的变换速率，transition-timing-function有6个可能值：

1、ease：（逐渐变慢）默认值，ease函数等同于贝塞尔曲线(0.25, 0.1, 0.25, 1.0).

2、linear：（匀速），linear 函数等同于贝塞尔曲线(0.0, 0.0, 1.0, 1.0).

3、ease-in：(加速)，ease-in 函数等同于贝塞尔曲线(0.42, 0, 1.0, 1.0).

4、ease-out：（减速），ease-out 函数等同于贝塞尔曲线(0, 0, 0.58, 1.0).

5、ease-in-out：（加速然后减速），ease-in-out 函数等同于贝塞尔曲线(0.42, 0, 0.58, 1.0)

6、cubic-bezier：（该值允许你去自定义一个时间曲线）， 特定的cubic-bezier曲线。 (x1, y1, x2, y2)四个值特定于曲线上点P1和点P2。所有值需在[0, 1]区域内，否则无效。

#### 4.transition-delay

transition-delay延迟开始过渡的时间，默认值是0，表示不延迟，立即开始过渡动作。单位是s秒或ms毫秒。

### 四.触发过渡的方式

常见的就是伪类触发:hover，:focus，:active，:checked等。还有例如@media媒体查询，根据设备大小，横屏竖屏切换时触发。还有如click，keydown等JS事件触发。页面加载也能触发就不一一列举了。总之过渡的本质是在时间段内平滑过渡属性值，与怎么触发没有关系。
