---
title: changes not staged for commit 错误的解决方法
date: 2019-07-16 16:41:02
tags: Git
categories: Git
---

> 今天想把本地的hexo文件夹提交到GitHub仓库，提交是成功了，但是发现主题文件夹themes/next文件为空，运行git status 时提示 changes not staged for commit:，下面记录一下解决的方法。

通过查询得出了问题的原因是：如果git仓库下面还有另外一个clone过来的git仓库，那么上传到仓库的时候，其中的另一个克隆过来的文件夹一定是空的：

![](https://yxyuxuan.github.io/Markdown-repository/images/2019-07-16-img.png)

#### 解决方法：

1. 首先删除themes/next文件夹中的 .git文件

2. 在hexo/根目录下删除仓库中的空文件夹

   ```
   //删除暂存区或分支上的文件
   git rm -r --cached "themes/next"
   
   git commit -m "remove empty themes/next"
   git push origin master
   ```

3. 在hexo/根目录下重新提交

   ```
   git add .
   git commit -m "repush"
   git push origin master
   ```

至此到GitHub仓库中就能看到themes/next文件夹不是空的了。

总结：因为 clone 下来的文件夹也是一个 git 仓库，因此正常的 git add . 是无法提交该文件夹下的文件的，所以我们要做的就是删除文件夹下的 .git 文件夹，这样就可以通过  git add . 提交。

