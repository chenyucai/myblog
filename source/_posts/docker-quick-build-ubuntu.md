---
title: Docker下快速创建ubuntu容器
date: 2018-12-12 17:24:24
tags:
---


### 1.从网易云上面拉取ubuntu的镜像
```
docker pull hub.c.163.com/public/ubuntu:16.04-tools
```

<!-- more -->

### 2.运行一个 ubuntu 容器，并进入交互模式
```
docker run -it --name ubuntu-16.04 hub.c.163.com/public/ubuntu:16.04-tools /bin/bash
```

### 3.在 ubuntu 容器中依次执行下面命令
```
apt-get update --fix-missing

apt-get install -y software-properties-common

add-apt-repository ppa:webupd8team/java

apt-get update

apt-get install -y oracle-java8-installer oracle-java8-set-default git maven curl iputils-ping

rm -rf /var/lib/apt/lists/*

echo -e ' #!/bin/bash \n ping 127.0.0.1' > /usr/local/bin/

chmod +x /usr/local/bin/P
```

### 4.测试 P 指令
```
P
[ctrl + c]
```

### 5.退出 ubuntu 容器
```
exit
```

### 6.查看 ubuntu-16.04 容器的运行状态，如果是 UP 则停止容器
```
docker ps -a
```
如果是 UP 则停止容器
```
docker stop ubuntu-16.04
```


### 7.提交 ubuntu 容器为 uj8m3g2 镜像 {version} 为你自己定义的版本号
```
docker commit ubuntu-16.04 uj8m3g2:{version}
```