---
title: Linux 常用命令
date: 2017-11-21 21:01:00
tags: [linux, shell]
---



``` javascript
// 远程复制文件，将指定IP服务器下的文件（夹）复制到当前服务器，需要输入指定服务器的Xftp连接密码
scp -r root@10.0.0.1:/opt/software/nginx /opt/software/						// 不需要制定端口
scp -P 12306(端口) -r root@10.0.0.1:/opt/software/nginx /opt/software/		// 需要指定端口
  
// 用来控制系统服务的实用工具，它以启动、停止、重新启动和关闭系统服务，还可以显示所有系统服务的当前状态
service nginx stop			// 停止服务
service nginx start			// 启动服务
service nginx restart		// 重启服务
service nginx status		// 查看服务状态
service status-all			// 显示所服务的状态
```
<!-- more -->
``` javascript
// 启动nginx
./nginx/sbin/nginx -s
// 重启nginx
./nginx/sbin/nginx -s reload

// 查看进程情况
ps -ef|grep nginx
// 关闭进程
kill -9 14929 (进程id)

// 安装rpm安装包
rpm -ivh your-package.rpm	// 其中your-package.rpm是要安装的rpm包的文件名，一般置于当前目录下

// 设置权限
r=读取属性　　//值＝4
w=写入属性　　//值＝2
x=执行属性　　//值＝1
chmod 777 h.txt

// 查看定时任务
crontab -l

// 解压文件
tar -xzvf file.tar.gz		// 解压tar.gz文件
unzip upms.war -d upms		// 解压.zip文件

// 创建文件夹
mkdir -p srv/www/app/test	// 若所要建立目录的上层目录目前尚未建立，则会一并建立上层目录

// 打开文件
tail -f nohup.out (文件名称)
vim nohup.out
cat nohup.out
more nohup.out

// 复制文件
cp file.txt /bak
cp -r /srv/www/app /app-bak

// 删除
rm file.txt					// 删除文件
rm -d file.txt				// 强制删除文件
rm -r /srv/www/app			// 将指定目录下的所有文件与子目录一并处理

// 上传文件
rz filename					// 上传文件到服务器

// 下载文件
sz filename					// 下载文件到本地

// vi 界面命令
:w							// 保存修改
:w new_filename				 // 保存为指定文件
:wq							// 保存修改并退出vi
ZZ							// 保存修改并退出vi
:q!							// 不保存退出vi
:wq!						// 保存修改并退出vi(文件所有者忽略文件的只读属性)
```

