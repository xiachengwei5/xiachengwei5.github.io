---
title: SVN 的标准目录结构 trunk、branches、tags
date: 2018-05-03 08:00:00
tags: [SVN, 版本控制, trunk, branches, tags]
---
SVN有自己的标准目录结构：trunk、branches、tags，实际使用场景如下：

> * truck(主干|主线|主分支)：是用来做主方向开发的，新功能的开发应放在主线中，当模块开发完成后，需要修改，就在branches中进行修改，修改完成后再合并到trunk中；
> * branches(分支)：分支开发和主线开发是可以同时进行的，也就是并行开发，分支通常用于修复bug时使用；一些阶段性的release版本也放在branches中，这些版本是可以继续进行开发和维护的；又比如为不同用户客制化的版本，也可以放在分支中进行开发；
> * tag(标记)：用于标记某个可用的版本，可以标记已经上线发布的版本，也可以标记正在测试的版本，通常是只读的。这里存储阶段性的发布版本，只是作为一个里程碑的版本进行存档；

<!-- more -->

SVN的这个标准的目录结构如下：

``` xml
projectName
+--branches
+++--branch1.0
+++--branch2.0（copy from tag_release_2.0）
+--tags
+++--tag_release_1.0
+++--tag_release_2.0
+--trunk
```



#### 1.通过eclipse打tag

![通过eclipse打tag1](http://olywxnzqu.bkt.clouddn.com/image/svn/svn_tag1.png)  

![通过eclipse打tag2](http://olywxnzqu.bkt.clouddn.com/image/svn/svn_tag2.png) 

![通过eclipse打tag3](http://olywxnzqu.bkt.clouddn.com/image/svn/svn_tag3.png)  



#### 2.通过eclipse打branch

操作方式与打tag相同，只是将资源库选择到branches中。





#### 3.合并分支代码

![合并分支代码](http://olywxnzqu.bkt.clouddn.com/image/svn/svn_tag4.png) 

![合并分支代码](http://olywxnzqu.bkt.clouddn.com/image/svn/svn_tag5.png) 



#### 4. 参考资料

[SVN trunk(主线) branch(分支) tag(标记) 用法详解和详细操作步骤](https://blog.csdn.net/vbirdbest/article/details/51122637)  