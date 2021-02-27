---
title: 最基本的HTML5网页骨架
date: 2016-08-01 23:35:55
tags: HTML5
categories: 前端
---

 ## 1、新的网页结构 ##

 ### 1.1 DOCTYPE声明 ####

HTML4中的DOCTYPE声明格式

    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

HTML5中的DOCTYPE声明格式

    <!DOCTYPE html>

### 1.2 网页字符编码 ###

在HTML4中的格式

    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />

在HTML5中的格式

    <meta charset=UTF-8" />

### 1.3 header元素 ###

   header元素表示页面中的一个内容区块或者整个页面的标题！

HTML5中使用方法：

    <header></header>

### 1.4 nav元素 ###

nav元素表示页面中的导航链接部分。

HTML5中使用方法：

    <nav>...</nav>

### 1.5 article元素

article元素表示页面中的一块玉上下文不相关的的独立内容，比如一篇文章中的文章。

HTML5的使用方法：

    <article></article>

### 1.6 section元素 ###

scection表示页面中的一块内容区块，比如章节的页眉、页脚等等。也可以和Hn（h1、h2..)  等一起  使用，标示出文档的结构！

    <section></section>

### 1.7 aside元素 ###
aside元素表示article元素的内容之外的，和内容相关的辅助信息！

    <aside><aside>

### 1.8 footer元素

footer表示页面或者是页面中的一块区域的页脚，比如存放文件的创建时间、作者、联系方式  等等。

    <footer></footer>

## 2、新增的主体结构元素 ##

### 2.1、article元素 ###

### 2.2、section元素 ###

HTML5轮廓工具https://gsnedders.html5.org/outliner/

### 3、aside元素 ###

### 4、nav元素 ###

使用场合:

    1、传统的导航栏
    2、侧边栏导航
    3、内页导航
    4、翻页操作
    5、time元素

Time元素代表24小时中的某个时间或者是日期，表示时间时允许带时差。

它可以定义的格式如下：

    <time datetime="2014-9-28">2014年9月28<time>
    <time datetime="2014-9-28">9月28<time>
    <time datetime="2014-9-28">今天的时间<time>
    <time datetime="2014-9-28T22:30">2014年9月28晚上10点<time>
    <time datetime="2014-9-28T22:30Z">UTC标准时间2014年9月2晚上10点<time>
    <time datetime="2014-9-28T22:30+8:00">中国时间2014年9月28晚上10点<time>

### 2.6、pubdate属性 ###

Pubdate表示出版或发表时间。

    <time datetime="2014-9-28" pubdate=”pubdate”>2014年9月28<time>

## 3、新增的非主体结构元素 ##

 ### 3.1、header元素 ###

       header元素是一种具有引导和导航作用的结构元素，通常用来放置整个页面或页面内的 一个内      容区块的标题，但也可以包含其他的内容，比如在header里面放置logo图片、搜索表单等等。

  注意：一个页面内并没有限制header的出现次数，也就是说我们可以在同一页面内，不同的内容区块上分别加上一个header元素。

  在HTML5中，一个header元素至少可以包含一个heading元素（h1-h6），也可以包括我们下节课将要学习的hgroup元素，还可以包括其他的元素，在新的W3C HTML5标准，我们还可以把NAV元素包括进去。

### 3.2、hgroup元素 ###

     hgroup元素是将标题和他的子标题进行分组的元素。hgroup元素一般会把h1-h6的元素进行分组， 比如在一个内容区块的标题和他的子标题算是一组。
     通常情况下，文章只有一个主标题时，是不需要hgroup元素的。

### 3.3、footer元素 ###

    footer元素可以作为他的上层父级内容区块或是一个根区块的注脚。footer元素一般情况下包括与 它相关的区块的注脚信息，比如作者、版权信息等。

 注意：footer元素和我们前面所讲的header元素一样，并没有限制他的个数。并且footer元素可以为article元素或者section元素添加footer元素。

### 3.4、address元素 ###

    address元素用来在页面中呈现联系信息，包括文档的作者、邮箱、地址、电话信息等。
    address元素还用来展示和文章中的相关的联系人的所有联系信息

## 4、其他新增和废除元素的认识 ##


### 4.1、新增的figure元素与figcaption元素 ###

      figure元素是一种元素的组合，带有可选标题。figure元素用来表示页面上一块独立的内容，如果将他从网页上删除不给我们的网页造成影响。
     figcaption元素表示figure元素的标题，它属于figure元素，figcaption元素必须书写在figure元素内部，可以写在figure元素内的其他从属元素前面或后面。一个figure元素内最多只允许放置一个figcaption元素。

### 4.2、新增的details元素与summary元素 ###

     details元素是一种用于标识该元素内部的子元素可以被展开、收缩显示的元素。details元素具有一个布尔类型的open属性，当该值为true时，该元素内部的子元素应该被展开显示，当该属性值为false时，其内部的子元素应该被收缩起来不现实。该属性的默认值为false，当页面打开时其内部的子元素应该处于收缩状态。
    summary元素从属于details元素，用鼠标点击summary元素中的内容文字时，details元素中的其他所有从属元素将会展开或者收缩。如果details元素内没有summary元素，浏览器那你会提供默认的文字以供点击，例如“details”
    <details>
    <summary
        html教程
    </summary>
    <p
        html是有麦子学院提供！www.maiziedu.com
    </p>
    </details>

### 4.3、新增的mark元素 ###

     mark元素表示页面中需要突出显示或高亮显示的，对于当前用户具有参考作用的一段文字。通常在引用原文时使用mark元素，目的是引起读者的注意。mark元素是对原文内容有补充作用的一个元素，他应该用在一段原文作者不认为重要的，但是现在为了与原文作者不相关的其他目的而需要突出显示或者高亮显示的文字上面。
    mark元素最主要的目的就是吸引当前用户的注意。
    mark元素除了高亮显示之外，还有一个作用就是在引用原文时，为了某种特殊的目的而把作者没有表示出来的内容重点表示出来。

### 4.4、新增的progress元素 ###

    progress元素代表一个任务的完成进度，这个进度可以使不确定的，表示进度z正在进行，但不清楚这个还有多少工作量没有完成，也可以用0到某个最大数字（比如100）之间的s数字来表示准确的进度情况（比如百分比）
    该元素具有两个表示当前任务完成情路昂的属性，value属性表示已经完成了多少工作量，max属性表示总共有多少工作量。工作量的单位是随意的，不指定的。
    在设定属性点时候，value属性和max属性只能指定为有效的浮点数，value属性必须大于0，且小于或等于max的属性，max的属性必须大于0。

### 4.5、新增的meter元素 ###

  meter元素表示规定范围内的数量值。

  meter元素有6个属性！

    value属性：在元素中特地地表示出来的实际值。该属性值默认为0，可以为该属性制定一个浮点小数值。
    min属性：指定规定范围时允许实用的最小值，默认0，在设定该属性时所设定的值不能小于0。
    max属性：指定规定的范围时允许使用的最大值，如果设定时该属性值小于min的值，那么把min属性的值视为最大值。max属性的默认值1。
    low属性：规定范围的下限值，必须小于或者等于high的值。
    high属性：规定范围的上限值。如果该属性值小于low属性的值，那么把low属性的值视为high属性的值，同样如果该属性的值大于max属性的值，那么把max属性的值视为high的值。
    optimum属性：最佳值属性值必须在min属性值与max属性值之间，可以大于high属性值。

### 4.6、废除元素 ###

     能使用CSS代替的元素： basefont、big、center、font、s、strike、tt、u
     不在使用frame框架 。对于frameset元素、frame元素与noframes元素，由于frame框架对页面可用性存在负面影响，在html5里面已经不支持frame框架，只支持iframe框架，同时废除以上这三个元素。
    只有部分浏览器支持的元素。对于applet元素、bgsound、blink、marquee元素，由于只有部分浏览器支持这些元素，特别是bgsound元素以及marquee元素，只被IE浏览器支持，所以在HTML5里面被废除！而applet元素可以由embed元素或者object元素代替，bgsound元素由audio元素代替，marquee可以使用javascript来代替！

  其他被废除的元素

    A、废除rb元素，使用ruby元素代替
    B、废除acronym元素，使用abbr元素代替
    C、废除dir元素，使用ul元素代替
    D、废除inindex元素，使用form元素与input元素相结合的方式代替
    E、废除listing元素，使用pre元素代替  
    F、废除xmp元素，使用code元素代替
    G、废除nextid元素，使用GUIDS代替
    H、废除plaintext元素，使用“text/plian” MIME类型代替
