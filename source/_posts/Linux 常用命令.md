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
ps -ef | grep nginx
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
```

