---
title: Git使用入门
date: 2017-07-26 23:02:55
tags: [Git, GitHub]
---

最近在修改mybatis-generator的插件，让自动生成的代码更满足符合开发的要求，因为是业余研究，经常需要下班后在自己电脑上来继续之前的工作，之前的状态是下班之前把在公司的电脑上写的代码拷到U盘，回去之后再把代码拷到自己电脑中替换掉之前的，某些特定情况的数据（如：数据库连接信息等）还需要根据环境来调整，每次搭建开发环境都需要较长时间，这样一折腾之后连开发的激情都没有了，为了解决这个问题就需要借助功能强大、人气颇旺的版本控制工具——Git来管理。

### 一、初始GitHub

Git 是一款免费、开源的分布式版本控制系统。而 GitHub 主要提供基于 git 的版本托管服务。也就是说现在 GitHub 上托管的所有项目代码都是基于 Git 来进行版本控制的，所以 Git 只是 GitHub 上用来管理项目的一个工具而已，GitHub 的功能可远不止于此。

### 二、Git安装

到[官网](https://git-for-windows.github.io/) 下载安装即可，安装成功后在桌面右键可以看到下图所示的`Git Gui Here` 和 `Git Bash Here` ：

![安装成功右键显示](img/show.png) 

需要在哪个文件夹下运行Git 打开该文件夹，右键 ==>  `Git Bash Here` 即可打开命令窗口，如下图：

![命令窗口](img/git_bush.png) 

### 三、常用命令列表

**git init** 

初始化 git 仓库

**git status** 

查看你当前 git 仓库的状态信息，如：文件修改、删除等的信息

**git add** 

将需要提交的文件放在缓存中，具体用法如下：

``` xml
git add README.md
```

如果需要提交的文件很多，对每个文件都这样处理太繁琐，可以通过如下命令一次性添加所有变更后的文件：

``` xml
git add --all
git add .
```

**git rm --cached** 

移除缓存

**git commit** 

提交在缓存中的文件，具体命令如下：

``` xml
git commit -m "提交说明"
```

**git push** 

将提交的信息推送到**默认** 的远程仓库

**git push -u origin master** 

将提交的信息推送到**指定** 的远程仓库



``` xml



git pull
git branch
git branch -r
git checkout source
git log
git merge
```



解决中文乱码问题：在git bush中右键，选择编码格式；