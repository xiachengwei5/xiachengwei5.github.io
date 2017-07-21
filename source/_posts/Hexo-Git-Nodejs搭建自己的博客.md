---
title: Hexo+Git/Coding+Node.js搭建自己的博客
date: 2016-07-03 21:34:41
tags: [Hexo,Git,博客]
---
## 前言
一直一来都想找平台来记录工作上的一些经验和生活中的一些感悟，之前也用过CSDN和博客园这些博客平台，虽然注册了就可以用，但是自带的富文本编辑器太不好用，样式也不好看，最让人反感的是有太多的广告让本就不干净的页面显得很杂很乱，所以用了一段时间就放弃了，但是上次看到小宇的博客感觉很炫、很Geek，很简洁，典型的互联网风格，所以决定自己也搭一个来玩玩。
<!-- more -->
经过前期了解之后才知道使用流行的Hexo+Git+Node.js搭建的自己的博客，他因为考虑到Git在国内的访问速度比较慢，所以托管到了国内的Coding上，我经过两天的学习，暂时把博客托管到Git上，如果访问速度确实比较慢后续也会考虑到Coding上。

因为要用到Node.js所以专门提前学了一点Nodejs的知识（发现一个比较好用的，适合入门级的学习网站：[菜鸟教程](http://www.runoob.com/)，提前了解一下还是又有必要的，虽然网上关与“Hexo+Git+Nodejs搭建自己的博客”的文章一搜一大把，但是都多多少少感觉缺点东西，所以写下这篇博客，一方面是为了记录总结，另一方面也让更多的跟我一样第一次接触Hexo的朋友快速高效的搭建自己的博客，就让这篇文章成为新平台的第一篇博客吧。

---

先来了解几个概念：

1. 什么是Hexo
  Hexo是一个快速、简洁且高效的博客框架,可以方便的生成静态网页托管在[github](https://github.com/)、[Heroku](https://www.heroku.com/)和[Coding](https://coding.net/)等代码托管平台上，绑定自己的域名，用markdown写文章。
  类似Hexo的博客框架还有很多如:[Jekyll](http://jekyll.bootcss.com/)、Octopress、Wordpress等。

2. 为什么要用Hexo来写博客，引用原作者的话来说:
> * 超快速度: Node.js 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染。
> * 支持 Markdown: Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插件。
> * 一键部署: 只需一条指令即可部署到 GitHub Pages,Heroku或其他网站。
> * 丰富的插件: Hexo 拥有强大的插件系统，安装插件可以让 Hexo 支持 Jade, CoffeeScript。

3. 另外,还有几个优点另Hexo这么风靡:
>* 易用。部署的话，就算是小白，也很简单，而且平时用到的命令仅需要四个命令即可,不像Jekyll需要很多繁琐的git命令：
>> - hexo new —— hexo n（新建文章）
>> - hexo generate —— hexo g（生成静态文件）
>> - hexo server —— hexo s（启动服务预览）
>> - hexo deploy —— hexo d（上传部署）
>> - hexo clean （清理已生成的静态文件）
>
>* 轻便. 文件少，小，易理解，方便自定义更改；
>* 用户多. 用户的量级虽然比不上Jekyll，但遇到什么问题基本在网上搜索出来就能搞定了。

## 在本地搭建部署hexo
好啦上面废了那么多话，下面开始正式搭建：
### 安装所需的环境和工具
#### 安装Git Bush
百度Git Bush，打开官网，点击下载安装，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/gitBush.jpg)
安装成功之后，打开安装好的的Git Bush，输入：
``` bush
$ git --version
```

查看安装的Git Bush的版本号，输出版本信息表示安装成功：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/gitVersion.png)
如果安装成功但是没有输出版本信息，需要自己Path配置环境变量，win10系统如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/gitEve.png)

#### 安装Node.js
百度node.js，进入官网，官网会自动检测系统版本提供可用版本供下载，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/nodeSite.png)
安装成功后，在Git Bush中输入：
``` bush
$ node -v
```
查看安装的node.js的版本号，输出版本信息表示安装成功，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/nodeVersion.png)
NPM是随同NodeJS一起安装的包管理工具，查看其版本信息，在Git bush中输入：
``` bush
$ npm -v
```
查看安装的npm的版本号，输出版本信息表示安装成功，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/npmVersion.png)
如果安装成功但是没有输出版本信息，需要自己Path配置环境变量，win10系统如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/nodeEve.png)

#### 通过Node.js命令安装Hexo
在Git Bush中输入：
``` bush
$ npm install hexo-cli -g
```
因暂时还不知道怎样通过命名在Git Bush中切换当前文件夹，所以直接在建好的博客根目录（我的是：D:\hexo），右键，选择“Git Bush Here”打卡Git Bush命令窗口，后续命令都在该命令窗口下输入，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/gitBushHere.png)
安装完成后，初始化Hexo，初始化完成后就会自动在目标文件夹下创建建立网站所需要的文件，网络不好时Cloing比较慢请耐心等待，输入：
``` bush
$ hexo init
```
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/hexoInit.png)

待输出“INFO String blog with Hexo!”表示已经初始化成功。
安装依赖包，输入：
``` bush
$ hexo install
```
安装成功后，目录结构如下：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/hexoInstall.png)
因Hexo 3.0以后把服务器独立成了单独的模块,所以还需要安装hexo-server才可以使用，输入：
``` bush
$ npm install hexo-server --save
```
### 本地调试
到目前为止，本地的Hexo博客已经搭好了，先生成文件，输入：
``` bush
$ hexo g
```
再启动服务器，输入：
``` bush
$ hexo s
```
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/hexoServer.png)
在浏览器中输入：http://localhost:4000/ ，查看初始页面，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/welcome.jpg)
好啦，今天就到此为止，本地的Hexo就已经部署完毕了，接下来就是要将博客部署上传到Git上，欲知详情，请听下回分解！

## 将hexo部署到服务器上
好咧，接着上次的说，在本地部署好之后还不过瘾，我们需要挂到网上让其可以通过互联网来访问，这时就要用到一些代码托管平台提供的pages服务（静态站点托管服务），我试了一下挂在Git上访问速度确实有点慢，所以最后还是选择挂在国内的Coding上，访问速度上会快很多，下面我们就把这两种方式都介绍一下：
### 将hexo挂在Git上
因为GitHub的网站是全英文的所以这个时候才觉得英文好其实对编程还是很有帮助的，有时其实还是蛮佩服何老师的
#### 注册GitHub
该操作比较简单，自己处理，不清楚的可以查看[示例教程](http://wiki.jikexueyuan.com/project/github-basics/sign-up.html)。
#### 创建项目
然后登录创建自己的仓库，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/github_home.jpg)
打开创建界面后，输入Repository name，注意：此处的Repository name最好与拥有者名称（用户名）保持一致，防止在访问时出现一些莫名其妙的问题，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/Repository_name.jpg)
点击“Create Repository”按钮创建repository，点击右边Settings，拉到最下方找到GitHub Pages模块，点击Launch automatic page generator，让GitHub生成GitHubPager，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/repositoryPages.jpg)
点击下面的“Continue to Layouts”进入下一步，下一个操作是选择主题，但选择哪个无所谓,因为后面将要与Hexo关联，站点所有内容都将被Hexo博客所替换，所以直接点击“Publish page”，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/publishPage.jpg)
点击“Settings”即可看到如下提示：Your sit is published at xiachengwei5.github.io，表示你可以访问独立的该域名的网站了，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/site_name.png)
#### 配置SSH Keys
下面只说明在本次处理中的操作，详细信息请查看[官方教程](https://help.github.com/articles/generating-an-ssh-key/)。
SSH密钥是一个用来识别值得信赖的电脑在进行GitHub一些操作时，不用输入密码。用户可以生成一个SSH密钥，并按照本节所述的方法将公共密钥添加到你的GitHub帐户。
生成新的SSH密钥并将其添加到ssh-agent中
打开gitBush，将后面的邮箱地址换成你的邮箱地址：
``` bush
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
当提示你，如下信息，按Enter键，表示接受默认文件位置：
``` bush
Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
```
当出现如下提示时，输入安全密码（可以为空），既然可以为空，那就为空吧，免得以后麻烦：
``` bush
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```
将ssh秘钥添加到ssh-agent，输入：
``` bush
$ eval $(ssh-agent -s)
```
如果你使用现有的SSH密钥，而不是生成新的SSH密钥，你需要替换现有的私有密钥文件的名称，以取代id_rsa的命令:
``` bush
$ ssh-add ~/.ssh/id_rsa
```
添加新的SSH密钥到你的帐户GitHub中
复制SSH密钥到剪贴板
``` bush
$ clip < ~/.ssh/id_rsa.pub
```
如果clip命令没有执行，你可以找到隐藏的.ssh文件夹中，打开你喜欢的文本编辑器文件，并将其复制到剪贴板,一般是在C:\Users\yourname.ssh文件夹下的id_rsa.pub文件中,使用文本文档打开后复制内容即可，我的文件路径为：C:\Users\xiachengwei\.ssh\id_rsa.pub
在GitHub任何界面中，点击右上角个人资料照片，选择Settings，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/gitSettings.jpg)
点击左边菜单中的“SSH and GPG keys”，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/ssh_key.jpg)
然后点击“New SSH Key”，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/new_ssh_key.jpg)
在“Title”字段中，为新的密钥添加描述性标签，粘贴刚才复制的秘钥值到“key”框中，点击“Add SSH key”，然后输入你的GitHub密码进行确认：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/add_ssh_key.jpg)
到此SSH密钥的添加就完成了。

---

## 在Hexo配置文件中关联服务器信息
### 在Hexo配置文件中关联GitHub账号
打开刚才所建的hexo的目录(H:\Hexo下)，编辑_config.yml，在最下方找到deploy标签，然后更为如下配置，注意:你自己在修改时，需要将 xiachengwei5修改为自己的用户名：
``` bush
deploy:
  type: git
  repo: ssh://github.com/xiachengwei5/xiachengwei5.github.io
  branch: master
  message: xiachengwei's blog
```
有几个需要注意的地方：
1.注意: 因yml格式要求，所有键值对的”:”冒号后面必须跟有一个空格；
2.注意: 自Hexo 3.0以后，type类型都为 git，而非 github；
3.repo指定Git的url地址时用的是 ssh而不是，http或https；

这样处理之后就完成了在Hexo配置文件中关联GitHub账号，下面执行命令生成静态网页：
``` bush
$ hexo g
```
再执行命令将静态网页部署到Github上，这时会提示输入github的邮箱和密码：
``` bush
$ hexo d
```
提交完成之后就可以通过：http://xiachengwei5.github.io 访问用hexo搭建的博客了。

但是部署到GitHub之后发现访问速度真的很慢，还经常出现访问不了的情况，所以又尝试将博客部署到国内的Coding上，部署到Coding上比部署到GitHub上简单些，不需要配置SSH密钥认证。
首先还是要注册[Coding](http://coding.net)帐号，这个是中文网站，自己搞定。
登录成功后，点击“创建新项目”，如下图：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/coding_add.jpg)
同样项目名与用户名保持一致（这样处理表示是“用户Pages”，可以通过：http://username.coding.me 访问），选择“公开”：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/coding_add_baseInfo.jpg)
开启Coding的pages服务，特别注意：部署分支默认为：coding-pages，但是新建项目的默认分支为：master，所以没有coding-pages分支的话这里需要写成master：
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/coding_pages.jpg)
![img](http://olywxnzqu.bkt.clouddn.com/img/hexo_create/coding_master.jpg)

### Hexo配置文件中关联Coding账号
同样修改hexo目录（H:\hexo）下的_config.xml文件中的deploy标签的信息：
``` bush
deploy:
  type: git
  repo: https://git.coding.net/xiachengwei5/xiachengwei5.git
  branch: master
  message: xiachengwei's blog
```
同样在执行命令之后生成静态文件然后部署到Coding上，就可以通过http://xiachengwei5.coding.me 访问搭建好的博客了：
``` bush
$ hexo g
$ hexo d
```
如果在执行`hexo d` 时出现`npm install hexo-deployer-git --save` 错误，运行：

``` shell
npm install hexo-deployer-git --save
```

---
写了这篇博客之后发现自己是个婆婆妈妈的人，在写的时候想把每一个细节都囊括其中，想让一个小白都能根据这篇文章都能搭建自己的hexo博客，但是最后回过头来看发现很累赘，所以以后的文章会精简，抓住关键的几个点，起点拨作用。

---
特别鸣谢：
[小宇](http://liuyy.coding.me/)给我的启发以及[SmallCheric](http://blog.csdn.net/smallcheric/article/details/51049683)的博客对我搭建博客上的帮助。
