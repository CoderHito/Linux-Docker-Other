##基本概念
###镜像
* 可以说是一个模板，里面封装了程序运行所需的环境，包括各种依赖、包
* 镜像可以用来创建容器
###容器
* 容器就是镜像的实例
* 每个容器都是相互分离的
###仓库
* 存放镜像的地方叫做仓库
* 公开和私有两种形式
	* DockHub

##安装Docker
###Ubuntu16.04安装
* 前提
	* update
	* upgrade
* root用户下运行`curl -sSL https://get.daocloud.io/docker | sh`


##获取镜像
* `docker pull`

##列出本地镜像
* docker images
* 查看日志
	* docker logs 68fd2b084e76
	* 68fd2b084e76 为容器id

* docker ps -a 
	* 列出所有容器，包括正在运行的和已经结束的


* 运行
	* ` sudo docker run [OPTIONS] IMAGE[:TAG] [COMMAND] [ARG...]`
*  OPTIONS总起来说可以分为两类：
	* 设置运行方式：
决定容器的运行方式，前台执行还是后台执行；
设置containerID；
设置网络参数；
设置容器的CPU和内存参数；
	* 设置权限和LXC参数；
设置镜像的默认资源，也就是说用户可以使用该命令来覆盖在镜像构建时的一些默认配置。
	
	* -d:让容器在后台运行。-rm不能同时使用。
	* -P:将容器内部使用的网络端口映射到我们使用的主机上。
	* -it：交互式操作

* `docker attach`
	* 当使用了-d运行在后台的时候，是用attach可重新附着到该容器。

##从daocloud拉取镜像
* 拉取镜像
	* docker pull daocloud.io/library/ubuntu

* 运行
	* `docker run -t -i ubuntu:16.04 /bin/bash`
	* 执行apt-get update 试试
	* 安装vim试试
	
* 启动、停止、重启
	* start、stop、restart + ID

* 更新与提交
	*  `docker commit -m="has update" -a="runoob" e218edb10161 ubuntu:v2`
	*  docker commit 
	*  -m="":提交的信息描述
	*  -a="":镜像作者
	*  e218edb10161：容器ID
	*  ubuntu:v2：指定要创建的目标镜像名