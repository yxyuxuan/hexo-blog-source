---
title: 记录工作中用到的Git命令
date: 2019-05-10 17:27:28
tags: Git
categories: Git
---

> 每天敲的代码都要提交到GitLab上，其中遇到过各种问题，这里简单做一下记录，后续会补充。

##### Clone repository：

```
git clone 项目链接
```

##### Git global setup:

```
git config --global user.name "于禤"
git config --global user.email "1924442613@qq.com"
```

##### Push an existing folder:

```
cd existing_floder
git init
git remote add origin 项目链接
git add .
git commit -m "inital commit"
git push -u origin master
```

##### Edit commit & git reset：

​	git commit --amend //此时会进入默认vim编辑器，修改注释完毕后保存就好了。

​	git reset --soft HEAD^ //不删除工作空间改动代码，撤销commit，不撤销git add .

​	git reset --hard HEAD^ //删除工作空间改动代码，撤销commit，撤销git add . 

​	git reset --mixed HEAD^ //不删除工作空间改动代码，撤销commit，并且撤销git add 		

​	git reset –soft commitid 后面添加comiit_id指明回退到哪个版本

​	git reset --mixed HEAD^ 和 git reset HEAD^ 效果是一样的

​	HEAD^的意思是上一个版本，也可以写成HEAD~1

​	如果你进行了2次commit，想都撤回，可以使用HEAD~2

##### 项目A的develop分支合并到项目B中:

```
//在项目B下添加远程项目A
git remote add upstream 项目A的链接

git fetch upstream 

//将项目A的develop分支合并到项目B中
git merge --no-ff --log upstream/develop
```

##### 项目A的某次提交合并到项目B中:

```
//在项目B下添加远程项目A
git remote add upstream 项目A的链接

git fetch upstream 
git cherry-pick 09cc4ecb0c25a1968633b27d3ae60940768f117d（commit id）
```

##### 合并分支：

(v0.18版本合并到develop开发分支)：

首先切换到v0.18分支，拉下最新的代码
然后切换到develop分支，拉下最新的代码然后执行下面的步骤：

	git merge --no-ff v0.18
	(如果没有冲突则不用进行下面的操作)
	
	解决冲突后执行以下命令： 
	git add .
	git commit  //进入另一个页面查看信息后退出按键组合是：Esc -> Shift -> : -> wq
	git push origin develop
##### 创建develop分支：

	git fetch
	git checkout -b develop
	git push origin develop
##### 解决提交代码冲突：

  第一步：找到冲突的文件，合并冲突的内容。
  第二步：重新提交一次  git add .
  第三步：查看状态  git status
  第四步：git rebase --continue
  第五步：git push origin v0.2

##### 解决pull代码时文件突然被删除的问题：

  第一步：关闭正在运行的项目
  第二步：终止pull命令， git rebase --abort
  第三步：重新git pull --rebase origin develop
  第四步：git push origin develop

##### git cherry-pick 的用法:

把0.12上的部分提交移到0.11上具体步骤：

1. git checkout v0.11

2. git pull --rebase origin v0.11

3. git cherry-pick a6b90cab238fcd04590011f7385b40d1e2171f10  (提交的id)

4. git status

   如果有冲突就解决冲突，解决冲突后执行以下步骤:

   ```
   git add . 
   git status
   git cherry-pick --continue
   git push origin v0.11	
   ```

    如果没有冲突，则执行

   ```
   git push origin v0.11 
   ```

   ##### git revert 的用法:

   > revert是撤回指定版本的内容并提交一个新的commit，不影响之前提交的内容

    git revert HEAD     //撤销前一次 commit

   git revert HEAD^   //撤销前前一次 commit

   git revert commit-id //撤回指定commit-id

   ##### git reset 和 git revert 的区别

   git reset 简单暴力的将版本置回到某个版本，现在有过a、b、c、d四次提交，提交顺序为a、b、c、d，现在为d。

   使用git reset恢复到a之后，看git log，就剩下a, 发现 b、c、d都不见了。

   使用git revert恢复到a之后，看git log，会发现a、b、c、d都在，多了e操作，e操作为“revert a”。

   