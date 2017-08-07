---
title: 通过Eclipse的Git插件维护GitHub上的代码
date: 2017-07-28 20:00:00
tags: [git插件, git, github, eclipse]
---
`Git` 命令是程序员必备的技能，但是在大型项目的开发过程中如果全部使用命令来进行版本控制还是很麻烦的，突出表现在更新/提交时要查看具体更新/修改了哪些内容；提交部分文件时选择文件的操作；查看解决冲突问题等；这时需要一个与 `IDE` 结合、提供可视化界面的插件来协助解决这个问题。

<!-- more -->

### 一、插件安装

现在较新版本的 `eclipse` 已经集成了 Git 插件，如果没有集成可以到 `Eclipse Marketplace` 搜索 `EGit` 进行安装：

![安装EGit插件](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/install_EGit.png) 

### 二、从远程仓库下载项目到本地


右键==> `import` ，选择`Projects form Git` 

![导入项目](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/import_git.png) 

![导入远程项目](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/import_git4.png) 

点击`Next` ，输入远程仓库的 URI 地址：

![导入远程项目](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/import_git5.png) 

点击`Next` ：

![导入远程项目](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/import_git6.png) 

选择导出项目所在的路径：

![导入远程项目](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/import_git7.png) 

![导入远程项目](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/import_git8.png) 

点击`Finish` ，然后把弹出的窗口都关掉，然后右键==>`import` ，选择`Existing Projects into Workspace` ：

![导入远程项目](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/import_git9.png) 

选择刚才导出的项目：

![导入远程项目](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/import_git10.png) 

### 三、将本地项目上传到远程仓库

当需要将本地的项目上传到远程仓库时，在项目上右键==> `Share Project` ，如下：

![Share Project](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/share_git1.png) 

展示电脑上已经安装的版本控制工具的插件，如下：

![选择版本控制工具](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/share_git2.png) 

选择 `Git` ，点击`Next` ，选择创建新的本地仓库，如下：

![选择创建新的本地仓库](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/share_git7.png) 

在项目上右键==> `Team` ==> `Commit` ，打开提交窗口，如下：

![提交文件](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/share_git8.png) 

此处点击 `Commit` ，**因为新建的仓库只有提交一次后才会有生成默认的分支，后续才能选择** 。在eclipse中的项目上右键，操作如下：

![与远程仓库建立联系](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/share_git4.png) 

 ![与远程仓库建立联系](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/share_git5.png) 

![与远程仓库建立联系](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/share_git6.png) 

点击 `Finish` 即与远程仓库建立了关联，可以进行下一步的提交代码了。



### 四、代码提交

在项目上右键==> `Team` ==> `Synchronize Workspace` ，查看本地仓库和远程仓库文件的差异。

![提交代码](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/commit_git1.png) 

可以看到本地仓库与远程仓库的差异情况，如下：

![查看文件差异](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/commit_git2.png) 

但是其中有些是我们不需要提交的内容，如：`target` 文件夹下的内容，这是需要通过在项目根目录下通过配置 `.gitignore` 来设置不需要提交的文件和文件夹；

![配置.gitignore文件](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/commit_git3.png) 

配置完成之后再查看差异可以看到不需要提交的 `target` 文件夹已经不在差异文件目录中了，说明已经被忽略，然后选中需要提交的文件，右键==> `Commit`  

![提交代码](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/commit_git4.png) 



![提交代码](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/commit_git5.png)  

提交完成之后返回提交的结果，如下：

![返回提交结果](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/commit_git6.png) 

到此代码已经提交成功了，可以到远程仓库查看，如下：
![查看远程仓库](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/commit_git7.png) 

### 五、代码下载

在项目上右键==> `Pull...` ，如下：

![更新代码](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/pull_git1.png) 

这里可以选择从哪个分支更新，配置完成后点击`Finish` ，*此处不考虑代码有冲突的情况，对有冲突的情况后续在补充* 。

![更新代码](http://olywxnzqu.bkt.clouddn.com/img/eclipse_git/pull_git2.png) 

### 六、参考资料

[eclipse提交本地项目到github](http://blog.csdn.net/bruce1225/article/details/44726797) 