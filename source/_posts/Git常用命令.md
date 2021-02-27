---
title: Git常用命令
date: 2016-10-12 19:52:30
tags: Git
categories: Git
---


> Git是目前世界上最先进的分布式版本控制系统，关于Git的常用命令整理如下：

### 1.创建版本库

    $ mkdir learngit
    $ cd learngit
    $ pwd
    /Users/michael/learngit
### 2.通过git init命令把这个目录变成Git可以管理的仓库，在新建版本库路径下：

    $ git init
    Initialized empty Git repository in /Users/michael/learngit/.git/
### 3.添加文件到Git仓库，分两步：

第一步，用命令git add告诉Git，把文件添加到仓库：

    $ git add readme.txt
执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令git commit告诉Git，把文件提交到仓库：

    $ git commit -m "wrote a readme file"
    [master (root-commit) cb926e7] wrote a readme file
     1 file changed, 2 insertions(+)
     create mode 100644 readme.txt
### 4.要随时掌握工作区的状态，使用git status命令。

    git diff readme.txt

如果git status告诉你有文件被修改过，可以用git diff readme.txt查看修改内容。
    

### 5.版本回退

    git log  //显示从最近到最远的提交日志，以便确定要回退到哪个版本，每次提交都有一个唯一的id。
    
    git reflog  //用来记录你的每一次命令，以便确定要回到未来的哪个版本。
    
    把当前版本“append GPL”回退到上一个版本“add distributed”，就可以使用git reset命令：
    $ git reset --hard HEAD^
    HEAD is now at ea34578 add distributed
返回某个指定id号的那个版本的提交：

    $ git reset --hard 3628164

查看readme.txt的内容是不是版本add distributed：

    $ cat readme.txt
    Git is a distributed version control system.
    Git is free software.
git 允许我们将某个特定的文件回滚到特定的提交，使用的也是 git checkout。

例子：将hello.txt回滚到最初的状态，需要指定回滚到哪个提交，以及文件的全路径。

    $ git checkout 09bd8cc1 hello.txt
### 6.撤销修改

    git checkout -- file  //丢弃工作区的修改：
    
    $ git checkout -- readme.txt
    命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
    一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
    一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
    总之，就是让这个文件回到最近一次git commit或git add时的状态。
    
    git add到暂存区了，git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区，
    git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

### 7.删除文件

一种是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：

    $ git rm test.txt
    rm 'test.txt'
    
    $ git commit -m "remove test.txt"
    [master d17efd8] remove test.txt
    1 file changed, 1 deletion(-)
     
    $ delete mode 100644 test.txt
    现在，文件就从版本库中被删除了。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

    $ git checkout -- test.txt
    
    git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

### 8.远程仓库

#### 第一种：添加远程库：

      1.mkdir learngit
    
      2.cd learngit
    
      3.git init
    
      4.添加文件
    
      5.git add .
    
      6.git commit -m "test"
    
      7.在github上创建仓库，复制地址
    
      8. git remote add origin git@github.com:michaelliao/learngit.git
    
      9.git push -u origin master

**注意：**

  若出错：fatal: refusing to merge unrelated histories

  解决：git pull origin master --allow-unrelated-histories

  //github仓库中存在本地项目中不存在的文件，发生了冲图，所以应该先将github上的仓库全部pull下来，让两个仓库相关联。

  最后依次用命令：

     git add .              
     git commit -m "commit2"                    
     git push origin master:master

#### 第二种：从远程克隆仓库

      1.创建仓库，复制地址
    
      2.克隆：git clone  git@github.com:michaelliao/learngit.git
    
      3.添加文件
    
      4.git add .
    
      5.git commit -m "test"
    
      6.git push -u origin master

### 9.分支管理

#### a.创建与合并分支：

      1.创建分支：git branch dev
    
      2.切换到分支：git checkout dev
    
      创建+切换分支：git checkout -b dev
    
      3.查看当前分支：gir branch 
    
      4.将指定分支合并到当前分支,假如当前分支为:master：git merge dev
    
      5.删除dev分支：git branch -d dev
    
      强行删除，需要使用命令:git branch -D dev。

#### b.解决冲突：

1. 用带参数的git log可以看到分支的合并情况：git log --graph --pretty=oneline --abbrev-commit
c.bug分支：修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。

2. 把当前工作现场“储藏”起来：git stash。现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。
  
3. 工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：
一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；另一种方式是用git stash pop，恢复的同时把stash内容也删了。工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看。

#### c.多人协作：

      1.查看远程库的详细信息：git remote -v
    
      2.推送分支：git push origin master
    
      3.抓取分支：

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

### 10.标签管理：
#### a.创建标签：
  首先，切换到需要打标签的分支上：git checkout branch-name然后打标签：git tag tag-name。git tag查看所有标签。

  如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

  方法是找到历史提交的commit id，然后打上就可以了： 

    $git log --pretty=oneline --abbrev-commit
    
    6a5819e merged bug fix 101
    cc17032 fix bug 1017825a50 merge with no-ff
    6224937 add merge
    59bc1cb conflict fixed
    400b400 & simple
    75a857c AND simple
    fec145a branch test
    d17efd8 remove test.txt
    ...
比方说要对add merge这次提交打标签，它对应的commit id是6224937，敲入命令：

    $ git tag v0.9 6224937
    
    再用命令git tag查看标签.
    git show <tagname>查看标签信息
    
    创建带有说明的标签，用-a指定标签名，-m指定说明文字：
    $ git tag -a v0.1 -m "version 0.1 released" 3628164
    
    通过-s用私钥签名一个标签：
    $ git tag -s v0.2 -m "signed version 0.2 released" fec145a
### 11.操作标签：
    命令git push origin <tagname>可以推送一个本地标签；
    
    命令git push origin --tags可以推送全部未推送过的本地标签；
    
    命令git tag -d <tagname>可以删除一个本地标签；
    
    命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

### 12.配置别名：
    $ git config --global alias.st status  //用st表示status
    
    然后用git st就可以查看状态。 

### 13.忽略文件：
大部分项目中，有些文件，文件夹是我们不想提交的。为了防止一不小心提交，我们需要gitignore文件：

在项目根目录创建.gitignore文件，在文件中列出不需要提交的文件名，文件夹名，每个一行，.gitignore文件需要提交，就像普通文件一样。

通常会被ignore的文件有：

1. log文件
2. task runner builds
3. node_modules等文件夹
4. IDEs生成的文件
5. 个人笔记
