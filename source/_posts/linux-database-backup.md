---
title: Linux下实现每天自动备份数据库
date: 2019-01-10 16:48:46
tags:
---

唠叨几句，土豪玩家都直接买的云数据库的服务，像我这种平民玩家也只好老老实实的写好脚本对数据库每天进行备份啦。

### 1 创建备份目录
```
$ cd /home
$ mkdir backup
$ cd backup
$ mkdir bks   #专门存放备份文件
```
注意：建议备份在空间充足的磁盘里，另外如果使用docker的话应该是备份在挂载的目录下。

<!-- more -->

### 2 创建备份脚本
```
$ vim backup.sh
```
脚本是这样的
```
#!/bin/bash
# a ShellScript for auto DB backup and delete old backup

backupdir=/home/backup/bks
time=`date +%Y%m%d%H%M%S`

mysqldump -uusername1 -ppassword1 database1 | gzip > $backupdir/database1_$time.sql.gz
mysqldump -uusername2 -ppassword2 database2 | gzip > $backupdir/database2_$time.sql.gz

find $backupdir -name "*.sql.gz" -type f -mtime +5 -exec rm {} \; > /dev/null 2>&1
```
解释一下脚本

name：自定义备份文件前缀标识。

-type f 表示查找普通类型的文件，f表示普通文件。

-mtime +5 按照文件的更改时间来查找文件，+5表示文件更改时间距现在5天以前；如果是 -mmin +5 表示文件更改时间距现在5分钟以前。

-exec rm {} \; 表示执行一段shell命令，exec选项后面跟随着所要执行的命令或脚本，然后是一对儿{ }，一个空格和一个\，最后是一个分号。

/dev/null 2>&1 把标准出错重定向到标准输出，然后扔到/DEV/NULL下面去。通俗的说，就是把所有标准输出和标准出错都扔到垃圾桶里面；其中的& 表示让该命令在后台执行。


### 3 给脚本添加执行权限
```
$ chmod 777 ./backup.sh
```

### 4 添加任务计划（定时任务）
先检测一下有没有安装 crontab
```
$ crontab  #如果没有安装会提示这个命令不存在
```
没有安装的安装一下
```
$ apt-get update
$ apt-get install cron
```
添加任务
```
$ crontab -e
```
然后再最后一行添加
```
00 8 * * * /home/backup/backup.sh
```
{% img /images/v2-0538f5a9ccc90209dc447bc6a6324d99_hd.jpg %}
启动crontab
```
$ service cron start
```
crontab基本命令
```
service cron status  #查看状态
service cron start   #启动
service cron restart #重启
service cron stop    #停止
```

<br>

### 5 参考
* {% link linux下如何实现mysql数据库每天定时自动备份 - 凌凌小博客 - CSDN博客 https://blog.csdn.net/qq_35923749/article/details/79363364 %}
* {% link linux下如何实现mysql数据库每天自动备份定时备份 - tonfay的博客 - CSDN博客 https://blog.csdn.net/tengfei_0812/article/details/62044130 %}

