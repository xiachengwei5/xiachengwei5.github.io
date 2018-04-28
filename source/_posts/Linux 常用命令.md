---
title: Linux 常用命令
date: 2017-11-21 21:01:00
tags: [linux, shell]
---



``` javascript
// 返回上层目录
cd ..
// 进入指定文件夹
cd nginx
// 查看当前文件夹的目录 
ll
// 打开文件
tail -f nohup.out (文件名称)		// 显示文件最后几行的数据
more nohup.out(文件名称)			// 分页打开文件信息，按Enter键加载更多



// 查看进程情况
ps -ef | grep nginx
// 关闭进程
kill -9 14929 (进程id)

// 启动nginx
./nginx/sbin/nginx -s
// 重启nginx
./nginx/sbin/nginx -s reload

// 启动项目
sh start.sh
// 关闭项目
sh stop.sh


// 解压
unzip upms.war -d upms		// 解压.zip/.war文件
tar -zxvf LibreOffice_6.0.0.3_Linux_x86-64_rpm.tar.gz		// 解压.tar.gz文件

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



upms部署环境地址：
/home/sushun/app/apache-tomcat-upms-8210/webapps
```

