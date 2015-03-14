---
layout: post
title: "docker 构建开发环境lnmp"
description: ""
category: 
tags: []
intro: "docker 构建开发环境lnmp"
---

## docker 简单说明

现在 docker 很火,因为是容器.具体的说明我就不说了
学习的地址 [docker入门](http://www.widuu.com/chinese_docker/index.html "docker入门")。 
现在网络上安装lnmp都是系统自带的。
下文主要讲述docker是基本centos6的源码安装

 * php+nginx+mysql+memcached 
 * mysql 
 
## 具体实现过程:

我们可以下载一个centos6

	docker pull centos:centos6

这里的centos6是一个Tag，类似于git的tag，这里的tag可以为你制定一个centos的版本。
如果你直接pull centos 他会下载一个最新centos下来，就是7版本
下载完成后，执行docker images命令可以列出你已经下载或者自己构建的image：

![docker images](http://felixanya.github.com/img/d_images.jpg)


你可以看到centos6的大小为206MB，以及它的IMAGE ID。 现在我们开始运行一个Container，命令很简单.
例如我们想运行一个执行Shell终端的Container来安装mysql和php

	docker run -it -v /usr/local/webserver/mysql:/usr/local/webserver/mysql -v /tmp:/tmp -v /home/felix/down:/data/tools/tmep centos:centos6 /bin/bash

你已经进入到一个Shell里面，可以执行你想执行的任何命令，就和在centos6里面一样，进去后默认是在根目录/下，可以看到经典的unix/linux目录结构，以及你所运行的bash版本等信息。其它的容器也是这样安装

安装完了重新封装Container
例如MYAQL

	docker commit CONTAINER ID local_mysql:v1

 例如PHP

	docker commit CONTAINER ID local_nginx:v1

## mysql安装过程要注意地方:
* 源码安装mysql  要开通172IP地址的访问，因为Container的地址172开头的，否则报mysql connect: Host '172.17.0.1' is not allowed
```
GRANT ALL PRIVILEGES ON *.* TO 'root'@'172.%' IDENTIFIED BY '123456'; FLUSH PRIVILEGES;
```

## php安装过程注意地方:
	./configure --prefix=/usr/local/webserver/php --with-mysql=/usr/local/webserver/mysql  --with-mysql-sock=/tmp/mysql.sock

mysql的启动

	docker run -it -v /usr/local/webserver/mysql:/usr/local/webserver/mysql -v /tmp:/tmp --name mysql -p 3306:3306 local_mysql:v1   /bin/bash


![docker images](http://felixanya.github.com/img/d_mysql.jpg)

web的启动

	docker run -it -v /usr/local/webserver/mysql:/usr/local/webserver/mysql -v /tmp:/tmp -v /home/share:/data/htdocs -p 82:80 --link mysql:mysql --name web  local_nginx:v1   /bin/bash


![docker images](http://felixanya.github.com/img/d_nginx.jpg)


![docker ps -a](http://felixanya.github.com/img/d_ps.jpg)

#### 为什么这么多目录挂载

   * 使用-v参数挂载mysql的unix sock目录，-v /tmp，这样可以直接在容器中使用mysql命令行连接
   * 使用-v参数挂载mysql的源码目录，-v /usr/local/webserver/mysql，这样可以让PHP的源码安装通过
   * 使用-v参数挂载PHP的源码目录，-v /home/share，这样可以让主机放源码程序


#### 这只是一个练习
构建基础Image最好用Dockerfile

数据库最好是用数据卷

其它日记也可以挂载的方式
