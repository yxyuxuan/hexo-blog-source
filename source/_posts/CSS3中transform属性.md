---
title: CSS3中transform属性
date: 2017-08-03 14:25:55
tags: CSS3
categories: CSS
---
### 一.Transform描述：

ransform是变形,改变的意思。在CSS3中transform主要包括以下几种：旋转rotate，扭曲skew,缩放scale和移动translate以及矩阵变形matrix。transform的作用也是改变，让元素倾斜，旋转，缩放，位移。

### 二.Transform语法：

transform: rotate | scale | skew | translate |matrix;

### 三.Transform子属性详解：

#### 1.transform:rotate(); 元素旋转

value(值)为旋转度数（deg），默认顺时针旋转。value若为负值则逆时针旋转。

使用如下：

	.box{
	    width:100px;height:100px;margin:20px auto;background-color:#75C934;
	    text-align:center;line-height:100px;font-size:18px;
	    /*过渡效果*/
	    -webkit-transition: transform 0.8s; 
	}
	.box:hover{
	    /*旋转80°*/
	    transform:rotate(80deg);
	}

#### 2.transform:skew();元素倾斜

value(值)为倾斜度数，value只有一个值，横向默认向左倾斜；有两个值，第二个值纵向默认向上倾斜。value若为负值则反方向倾斜。

使用如下：

    .box{
        width:100px;height:100px;margin:20px auto;background-color:#75C934;
        text-align:center;line-height:100px;font-size:18px;
        -webkit-transition: transform 0.8s,border-radius 0.8s; 
        border-radius:30px;
    }
    .box:hover{
        transform:skew(20deg,20deg);
        border-radius:0px;
   	}

### 3.transform:scale();元素缩放

value(值)为缩放倍数。value只有一个值，默认整体缩放；有两个值，第一个值横向缩放，第二个值纵向缩放。value值大于1放大，小于1大于0缩小，负值则反向缩放(元素呈镜像)

使用如下：

    .box{
        -webkit-transition: transform 0.8s,border-radius 0.8s;  
    }
    .box:hover{
        transform:scale(2,0.8);
        border-radius:40px;
    }

#### 4.transform:translate();元素位移

value(值)为位移像素，value只有一个值，横向默认向右移动；有两个值，第二个值纵向默认向下移动。value若为负值则反方向移动。

使用如下：

   	.box{
        -webkit-transition: transform 0.3s,box-shadow 0.3s;
     }
    .box:hover{
        transform:translate(-3px,-3px);
        box-shadow:3px 3px 5px 0px #000;
    }