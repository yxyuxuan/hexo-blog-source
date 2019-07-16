---
title: 关于hexo的使用
date: 2016-07-1 21:00:30
tags: hexo
categories: hexo
---


## 执行顺序
**执行以下hexo 命令需要在yxyuxuan.github.io/source/_posts文件夹下**

1.新建md文件(不用加.md后缀）

    hexo new 文件名

2.生成html文件

	hexo g    
3.在本地看看效果

	hexo s(此步操作可以忽略)
4.部署

	hexo d

## **请看解释：**

1.在yxyuxuan.github.io/source/_posts文件夹中新建md文件(不用加.md后缀)
hexo new 文件名

打开文件，将要写的内容放在里面。

---
title: test

date: 2016-09-09 22:15:07

tags:

---
> 注：
> 
> title:test就是你博客的标题，可以修改的
> 
> 每个冒号后面都有个空格

2.删除一篇文章

- 命令：<font color="red">hexo clean test.md</font>

- 命令：<font color="red">rm test.md</font>

- 如果不删除md文件，可以将md文件移到其他位置存储


3.在本地查看效果

	命令：hexo s
访问 http://localhost:4000 ，此时只有自己能看到效果，因为还得将生成的html文件部署到你的博客上。

4.部署(只有部署后，我访问你的博客地址，才能看到你写的文章)

    命令：hexo d

