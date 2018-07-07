---
title: Linux 常用命令
date: 2017-11-21 21:01:00
tags: [linux, shell]
---



``` javascript
// 从远程服务器复制文件
scp -r root@10.0.0.1:/opt/software/nginx /opt/software/				// 默认22端口不需要指定端口
scp -P 12369(端口) root@10.0.0.1:/opt/software/nginx /opt/software/	// 需要指定端口
scp -r conf 10.0.0.1:/opt/software/nginx							// 将文件复制到当前目录

// 控制系统服务的启动、停止、重新启动和关闭系统服务，还可以显示所有系统服务的当前状态
service nginx status			// 查看指定服务的状态
service --status-all			// 查看所有服务的状态
service nginx stop				// 停止服务
service nginx start				// 启动服务
service nginx restart			// 重启服务
```

<!-- more -->

``` javascript
// 查看指定应用的进程情况
ps -ef|grep nginx
ps -ef|grep ytgH5
// 关闭进程
kill -9 14929 (进程id)

// 解压
unzip upms.war -d upms			// 解压.zip/.war文件到指定文件夹
tar -zxvf filename.tar.gz		// 解压.tar.gz文件


// 打开文件
tail -f filename.txt			// 显示文件最后几行的数据
more filename.txt				// 分页打开文件信息，按Enter键加载更多
cat filename.txt				// 显示所有内容

// 新建文件夹
mkdir -p /srv/www/app/test		// 若所要建立目录的上层目录目前尚未建立，则会一并建立上层目录
mkdir -p-m 750 bin/os_1			// 创建文件夹并设置权限

// 安装rpm软件包
rpm -ivh package.rpm			// package.rpm是你要安装的rpm包的文件名，一般置于当前目录下

// 启动nginx
./nginx/sbin/nginx -s
// 重启nginx
./nginx/sbin/nginx -s reload

// 提交和管理用户的需要周期性执行的任务
crontab -l						// 列出该用户的计时器设置；

// 保存并退出正在vi界面编辑的文件
:w					// 保存修改
:w new_filename		 // 保存为指定文件
:wq					// 保存修改并退出vi
ZZ					// 保存修改并退出vi
:q!					// 不保存修改并退出vi
:wq!				// 强制保存修改并退出vi（文件所有者忽略文件的只读属性）

// 删除文件
rm 文件名称
// 删除文件夹
rm -rf 文件夹名称

// 上传文件
rz 选择文件
// 下载文件
sz 文件名称

// 启动redis
./redis-server redis.conf
// 连接redis客户端
./redis-cli -p 8200
// 查看所有的key值
keys *
  
  
// 登录远程主机(查看端口通讯是否正常)只有当程序用到了具体的端口才能telnet通
telnet 192.168.37.6 8080

// 查看端口是否被占用
netstat -anp|grep 80
lsof -i:80


// 变更文件或目录的权限
权限范围的表示法如下：
u User，即文件或目录的拥有者；
g Group，即文件或目录的所属群组；
o Other，除了文件或目录拥有者或所属群组之外，其他用户皆属于这个范围；
a All，即全部的用户，包含拥有者，所属群组以及其他用户；
r 读取权限，数字代号为“4”;
w 写入权限，数字代号为“2”；
x 执行或切换权限，数字代号为“1”；
- 不具任何权限，数字代号为“0”；
s 特殊功能说明：变更文件或目录的权限。

r=读取属性　　//值＝4
w=写入属性　　//值＝2
x=执行属性　　//值＝1

chmod u+x,g+w f01　　		//为文件f01设置自己可以执行，组员可以写入的权限
chmod u=rwx,g=rw,o=r f01
chmod 764 f01
chmod a+x f01　　			//对文件f01的u,g,o都设置可执行属性

// 加载文件系统到指定的加载点
// 挂载跨服务器文件
mount 192.168.37.6:/srv/www/app/upload /srv/www/app/upload -nolock -t nfs	// 建立挂载
umount /srv/www/app/upload												// 取消挂载
设置开机启动挂载：把mount 的命令放到/etc/rc.d/rc.local 里面去，保存退出就好了

// 设置硬件时间
hwclock --set --date '2018-06-06  21:27:00' 
// 设置系统时间和硬件时间同步
hwclock  --hctosys
```



#### 参考资料

[Linux命令大全](http://man.linuxde.net/) 

[Linux下nfs+rpcbind实现服务器之间的文件共享（mount 挂载）](https://blog.csdn.net/qq_30815327/article/details/78436445) 