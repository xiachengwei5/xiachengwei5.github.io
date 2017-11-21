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
tail -f nohup.out (文件名称)

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
```

