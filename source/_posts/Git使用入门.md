---
title: Git使用入门
date: 2017-07-26 23:02:55
tags: [Git, GitHub]
---

最近在修改mybatis-generator的插件，让自动生成的代码更满足符合开发的要求，因为是业余研究，经常需要下班后在自己电脑上来继续之前的工作，之前的状态是下班之前把在公司的电脑上写的代码拷到U盘，回去之后再把代码拷到自己电脑中替换掉之前的，某些特定情况的数据（如：数据库连接信息等）还需要根据环境来调整，每次搭建开发环境都需要较长时间，这样一折腾之后连开发的激情都没有了，为了解决这个问题就需要借助功能强大、人气颇旺的版本控制工具——GitHub来管理。

<!-- more -->

### 一、简介 

Git 是一款免费、开源的分布式版本控制系统。而 GitHub 主要提供基于 git 的版本托管服务。也就是说现在 GitHub 上托管的所有项目代码都是基于 Git 来进行版本控制的，所以 Git 只是 GitHub 上用来管理项目的一个工具而已，GitHub 的功能可远不止于此。

### 二、Git安装

到[官网](https://git-for-windows.github.io/) 下载安装即可，安装成功后在桌面右键可以看到下图所示的`Git Gui Here` 和 `Git Bash Here` ：

![安装成功右键显示](http://olywxnzqu.bkt.clouddn.com/img/git_door/show.png) 

需要在哪个文件夹下运行Git 打开该文件夹，右键 ==>  `Git Bash Here` 即可打开命令窗口，如下图：

![命令窗口](http://olywxnzqu.bkt.clouddn.com/img/git_door/git_bush.png) 

### 三、常用命令列表

#### 基础命令 

``` xml
<!-- 初始化 git 仓库 -->
git init

<!-- 与远程 git 仓库关联 -->
git remote add origin git@github.com:xiachengwei5/spring-mvc.git

<!-- 查看你当前 git 仓库的状态信息，如：文件修改、删除等的信息 -->
git status

<!-- 将需要提交的文件放在缓存中 -->
git add README.md
<!-- 如果需要提交的文件很多，对每个文件都这样处理太繁琐，可以通过如下命令一次性添加所有变更后的文件 -->
git add --all
git add .
git add *

<!-- 移除缓存 -->
git rm --cached

<!--对暂时不需要提交的文件进行暂存 -->
git stash
<!--恢复暂存文件 -->
git stash pop

<!-- 提交在缓存中的文件 -->
git commit -m "提交说明"

<!-- 将提交的信息推送到默认的远程仓库 -->
git push
<!-- 将提交的信息推送到指定的远程仓库 -->
git push -u origin master

<!-- 从默认分支更新文件 -->
git pull
<!-- 从指定分支更新文件 -->
git pull origin master

<!-- 查看标签 -->
git tag
<!-- 新建标签 -->
git tag v1.0
<!-- 切换到指定标签 -->
git checkout v1.0

<!-- 查看日志 -->
git log
```

#### 分支

``` xml
<!-- 查询本地的分支情况 -->
git branch
<!-- 查询远程仓库的分支情况 -->
git branch -r

<!-- 在本地新建分支 -->
git branch 分支名称
<!-- 将本地分支提交到远程服务器 -->
git push --set-upstream origin 分支名称

<!-- 删除本地分支 -->
git branch -d 分支名称
<!-- 删除远程仓库分支 -->
git push origin :分支名称
<!-- 强制删除本地分支 -->
git branch -D
```

#### 切换 

切换到指定分支、标签，或撤销还没有 add 进暂存区的文件，具体用法如下：

``` xml
<!-- 切换当前分支为source -->
git checkout source
<!-- 新建一个a分支，并且自动切换到a分支 -->
git checkout -b a

<!-- 切换到指定标签 -->
git checkout v1.0

<!-- 撤销a.md -->
git checkout a.md
```

#### 合并

合并分支，一般是在master分支下合并其他分支，具体用法如下：

``` xml
<!-- 直接将两个分支合并，合并之后两块还是相对独立的 -->
git checkout master
git merge source

<!-- 根据相关逻辑将两个分支合并，合并之后两个分支是糅合的 -->
git checkout master
git rebase source
```

#### 别名

对使用很频繁，并且命名比较长的操作每次输入都比较麻烦，可以通过`alias` 来起简单好记的别名：

``` xml
<!-- 表示 co 等同于 checkout -->
git config --global alias.co checkout
<!-- 可以根据习惯来定制一些组合 -->
git config --global alias.psm 'push origin master'

<!-- 格式化日志信息 -->
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)% d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"
```

#### 差异

``` xml
<!-- 查看当前文件与暂存区文件的差异 -->
git diff
<!-- 比较两次提交之间的差异 -->
git diff <$id1> <$id2>
<!-- 在两个分支之间比较 -->
git diff origin/master origin/source
<!-- 比较暂存区和版本库差异 -->
git diff --staged
```

#### 设置用户名和邮箱

``` xml
git config --global user.name "xiachengwei5"
git config --global user.email "xiachengwei5@163.com"
```

#### 其他命名

``` xml
<!-- 开启Git着色 -->
git config --global color.ui true
```

#### 四、解决中文乱码的问题 

**解决通过git status查看中文文件名乱码** 

``` xml
git config --global core.quotepath false
```

在git bush中右键==>`options` ，选择编码格式：

![选择字体和编码格式](http://olywxnzqu.bkt.clouddn.com/img/git_door/selectFont.png) 

### 五、参考资料

[从0开始学习 GitHub 系列](http://stormzhang.com/github/2016/06/19/learn-github-from-zero-summary/) 

[从0开始学习 GitHub 系列——电子书下载](http://pan.baidu.com/s/1miJYaYs) 