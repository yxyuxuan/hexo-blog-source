---
title: Github-README中展示Demo
date: 2016-07-02 09:39:12
tags: GitHub
---


## 步骤：

### 第一步：新建一个仓库


![](https://yxyuxuan.github.io/Markdown-repository/images/1.png)


### 第二步：找到Settings

![](https://yxyuxuan.github.io/Markdown-repository/images/2.png)


### 第三步：找到GitHub Pages

![](https://yxyuxuan.github.io/Markdown-repository/images/3.png)
![](https://yxyuxuan.github.io/Markdown-repository/images/4.jpg)  


### 第四步：查看分支

![](https://yxyuxuan.github.io/Markdown-repository/images/5.png)


 注意：我们生成githubPages的目的就是需要生成一个gh-pages分支(正常情况下只有一个master分支)

### 第五步：将远程仓库克隆到本地

![](https://yxyuxuan.github.io/Markdown-repository/images/6.png)

    git clone git@github.com:yxyuxuan/test.git

### 第六步：将分支切换到gh-Pages

    cd test （进入到你克隆仓库的本地文件夹） 
    
    git checkout gh-pages（将master分支切换到gh-pages分支上）

### 第七步：重新上传到github上

注意：将本地克隆的文件删了，只留下.git,然后把你想要展示demo页面相关的文件粘进去 
接着，执行以下语句 

    git add . （将本地所有文件加到仓库里） 
    git commit -m “message” （设置提交信息） 
    git push -u origin gh-pages （push文件到仓库中）

### 第八步：预览 

    https://yxyuxuan.github.io/test/index.html

