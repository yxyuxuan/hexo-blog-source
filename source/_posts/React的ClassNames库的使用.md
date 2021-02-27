---
title: React的ClassNames库的使用
date: 2018-03-11 23:24:38
tags: React
categories: React
---

当react原生动态添加多个className时就会报错，这时我们就可以利用classnames库添加多个className

安装： npm install classnames

功能： 简单来说就是将true的class 显示出来，false 的class 隐藏

例：

    import classnames from 'classnames'
    
    <div className=classnames({
    	'class1': true,
    	'class2': true
    	)>
    </div>	

用法：

    classNames('class1', 'class2'); // => 'class1 class2'
    classNames('class1', { class2: true }); // => 'class1 class2'
    classNames({ 'class1-class2': true }); // => 'class1-class2'
    classNames({ 'class1-class2': false }); // => ' '
    classNames({ class1: true }, { class2: true }); // => 'class1 class2'
    classNames({ class1: true, class2: true }); // => 'class1 class2

传入数组对象

    var arr = ['b', { c: true, d: false }];
    classNames('a', arr); // => 'a b c'

传入动态class

    let buttonType = 'primary';
    classNames({ [`btn-${buttonType}`]: true })
