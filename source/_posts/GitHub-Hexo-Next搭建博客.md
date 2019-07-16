---
title: GitHub+Hexo+Next搭建博客
date: 2019-07-16 09:55:42
tags: hexo
categories: hexo
---

> 最近重新整理和利用最新版本的hexo+next重构了一下我 的博客，下面简单记录了搭建博客的完整过程。

## 一、环境准备

1.  Node.js，[点击下载安装](https://nodejs.org/en/)，因为Hexo需用通过npm安装，而npm需要node，只要安装node 就会自带npm
2.  Git, [点击下载安装](https://gitforwindows.org/)，[这篇博客有Git的常用命令解析]([https://yxyuxuan.github.io/2016/10/12/Git%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4/](https://yxyuxuan.github.io/2016/10/12/Git常用命令/))
3.  Github账号，如果没有账号，[点击这里注册](https://github.com/)

## 二、在GitHub上创建Github Pages项目

#### 创建新仓库

 ![](<https://yxyuxuan.github.io/Markdown-repository/images/new-reponsitory.png>)

创建一个名称为`yourusername.github.io`的新仓库即可。这边的`yourusername`填写自己的用户名。Github会识别并自动将该仓库设为Github Pages。

然后设置github用户和邮箱：

```
git config --global user.name "your name"
git config --global user.name "your email"
```

#### 生成SSH密钥

执行以下命令：

`ssh-keygen -t rsa -C "Github的注册邮箱地址"`

然后，在`C:\Users\ASUS\.ssh`目录会有两个文件**id_rsa**和**id_rsa.pub**,打开**id_rsa.pub**，复制里面的所有内容到 [SSH keys这里](https://github.com/settings/keys) 的Key，Title随便填，然后`Add SSH key`就可以了

## 三、安装Hexo

[Hexo](https://hexo.io/zh-cn/docs/) 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

首页进入任意磁盘创建文件夹，这里文件夹命名为hexo,在该文件夹下鼠标右键选择Git Bash Here，执行命令：

```
$ npm install -g hexo-cli
```

接着执行：

```
hexo init
npm install
```

新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source  
| 	├── _drafts   
|	└── _posts
└── themes
```

`_config.yml`文件中填上博客的基本信息，网站的 [详细配置点击这里查看](https://hexo.io/zh-cn/docs/configuration) 。注意，冒号后面都要有一个空格。

## 四、部署

在`_config.yml`文件，找到`deploy`，进行以下配置：

```
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: master
```

安装部署工具

```
npm install hexo-deployer-git --save
```

新建一篇博客文章，在hexo/source/_posts文件夹下新建md文件(不用加.md后缀）

```
hexo new 文件名
```

最后输入以下命令生成网站文件并部署：

```
hexo g //生成网页文件
hexo s //localhost:4000本地预览效果
hexo d //部署
```

在浏览器输入`https://username.github.io/`就能看到博客网站了。

## 五、用Next美化博客

Hexo安装后，默认主题是[landscape](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fhexojs%2Fhexo-theme-landscape)，这款主题相信你不会很喜欢，接下来讲解目前很火的一款主题-[NexT主题](https://github.com/theme-next/hexo-theme-next)的使用。

在hexo文件夹下鼠标右键选择Git Bash Here，然后clone next主题：

```
git clone https://github.com/iissnan/hexo-theme-next themes/next更
```

更新主题NexT：

```
cd themes/next
git pull
```

切换成NexT主题，在hexo根文件夹下，修改`_config.yml`文件中的theme：

```
theme: next

//切换后，用命令清除下缓存
hexo clean

//执行hexo s本地产看NexT主题效果
hexo s
```

切换next主题的风格，修改`hexo/theme/next/_config.yml`

```
# Schemes
#scheme: Muse
#scheme: Mist
scheme: Pisces
#scheme: Gemini
```

设置Menu

默认只有首页和归档，如果还要添加，编辑`hexo/themes/next/_config.yml` 

```
menu:
  home: / || home  //首页
  about: /about/ || user  //关于
  tags: /tags/ || tags  //标签
  categories: /categories/ || th   //分类
  archives: /archives/ || archive //归档
  schedule: /schedule/ || calendar   //日程表
  sitemap: /sitemap.xml || sitemap   //站点地图
```

##### 创建分类文件夹：

```
hexo new page categories
成功后输出：INFO  Created: ~/Documents/blog/source/categories/index.md
```

根据上面的路径，找到`index.md`这个文件，打开后并添加`type: "categories"`：

```
---
title: 文章分类
date: 2019-07-15 23:30:33
type: "categories"
---
```

然后给文章添加“categories”属性

```
---
title: GitHub+Hexo+Next搭建博客
date: 2019-07-15 23:43:57
categories: hexo
---
```

##### 创建标签文件夹

```
hexo new page tags
成功后输出：INFO  Created: ~/Documents/blog/source/tags/index.md
```

根据上面的路径，找到`index.md`这个文件，打开后并添加`type: "tags"`，保存。

```
---
title: 文章分类
date: 2019-07-15 23:54:22
type: "tags"
---
```

然后给文章添加“tags”属性

```
---
title: GitHub+Hexo+Next搭建博客
date: 2019-07-15 23:43:57
tags: hexo
---
```

根据以上步骤就可以搭建出一个属于自己的博客网站了，赶紧行动吧！

