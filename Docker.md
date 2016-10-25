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
		* -t  选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上，  
		* -i  则让容器的标准输入保持打开。

* `docker attach`
	* 当使用了-d运行在后台的时候，是用attach可重新附着到该容器。

##从daocloud拉取镜像
* 拉取镜像
	* docker pull daocloud.io/library/ubuntu

* 运行
	* `docker run -t -i ubuntu:16.04 /bin/bash`
	* 执行apt-get update 试试
	* 安装vim试试 apt-get install -y vim
	
* 启动、停止、重启
	* start、stop、restart + ID

* 更新与提交
	*  `docker commit -m="has update" -a="runoob" e218edb10161 ubuntu:v2`
	*  docker commit 
	*  -m="":提交的信息描述
	*  -a="":镜像作者
	*  e218edb10161：容器ID
	*  ubuntu:v2：指定要创建的目标镜像名



##数据卷/数据卷容器

* 如果容器之间需要共享一些持续更新的数据，最简单的方式就是是用户数据卷容器，数据卷容器就是一种普通容器，专门提供数据卷供其它容器挂载使用。
* 创建数据卷容器dbdata
	* docker run -it -v /dbdata:/dbdata --name dbdata ubuntu:v3
* 创建db1和db2两个容器，并使用--volumes-from挂载dbdata容器中的数据卷
	* docker run -it --volumes-from dbdata --name db1 ubuntu:v3
	* docker run -it --volumes-from dbdata --name db2 ubuntu:v3

* 这样三个容器任何一个容器在该目录下写入，其它容器都能看见。

##Dockerfile
[https://blog.tankywoo.com/docker/2014/05/08/docker-2-dockerfile.html](https://blog.tankywoo.com/docker/2014/05/08/docker-2-dockerfile.html)
[http://blog.csdn.net/we_shell/article/details/38445979#](http://blog.csdn.net/we_shell/article/details/38445979#)
###四部分
* 基础镜像信息
* 维护者信息
* 镜像操作指令
* 容器启动时执行指令

###dockerfile的执行流程
* docker从基础镜像运行一个容器
* 执行一条指令，对容器做出修改
* 执行类似commit的指令，提交一个新的镜像层
* 在基于刚提交的镜像运行一个容器
* 执行dockerfile的下一条指令，直到所有的指令都执行完成

###dockerfile的指令根据作用可分为两种：构建指令和设置指令
* 构建指令用于构建image
* 设置指令用于设置image的属性，其指定的操作将在运行image的容器中执行

**FROM 指定基础image**

* 构建指令
* 此指令在其他所有指令的前面，后续的指令都依赖于该指令指定的image
* 可以是官方远程仓库中的，也可以位于本地仓库

**MAINTAINER 指定镜像创建者信息**

* 构建指令
* 用于将image的制作者相关的信息写入到image中

**RUN 安装软件用**

* 构建指令
* RUN可以运行任何被基础image支持的命令
* 两种格式

	    RUN <command> (the command is run in a shell - `/bin/sh -c`)
	    RUN ["executable", "param1", "param2" ... ]  (exec form)

**CMD 设置container启动时执行的操作**

* 设置指令
* 该指令只能在文件中存在一次，如果有多个，则只执行最后一条
* 三种格式

	    CMD ["executable","param1","param2"] (like an exec, this is the preferred form)
	    CMD command param1 param2 (as a shell)

当Dockerfile指定了ENTRYPOINT

	    CMD ["param1","param2"] (as default parameters to ENTRYPOINT)  
ENTRYPOINT指定的是一个可执行的脚本或者程序的路径，该指定的脚本或者程序将会以param1和param2作为参数执行。所以如果CMD指令使用上面的形式，那么Dockerfile中必须要有配套的ENTRYPOINT。

**ENTRYPOINT设置container启动时执行的操作**

*  设置指令
*  指定容器启动时执行的命令，可以多次设置，但是只有最后一个有效。
*  两种格式
		
		ENTRYPOINT ["executable", "param1", "param2"] (like an exec, the preferred form)  
		ENTRYPOINT command param1 param2 (as a shell)  
* 该指令的使用分为两种情况，一种是独自使用，一种是和CMD指令配合使用
* 当独自使用时，如果还使用了CMD命令并且CMD命令是一个完整的可执行的命令，那么这两个指令会相互覆盖，只有最后一个有效
		
		# CMD指令将不会被执行，只有ENTRYPOINT指令被执行  
		CMD echo “Hello, World!”  
		ENTRYPOINT ls -l  

* 另一种用法和CMD指令配合使用来指定ENTRYPOINT的默认参数，这时CMD指令不是一个完整的可执行命令，仅仅是参数部分；ENTRYPOINT指令只能使用JSON方式指定执行命令，而不能指定参数。
		
		FROM ubuntu  
		CMD ["-l"]  
		ENTRYPOINT ["/usr/bin/ls"]

**USER 设置container容器的用户**

* 设置指令
* 设置启动容器的用户，默认是root

**EXPOSE 指定容器需要映射到宿主机器的端口**

* 设置指令，该指令会将容器中的端口映射成宿主机器中的某个端口。当你需要访问容器的时候，可以不是用容器的IP地址而是使用宿主机器的IP地址和映射后的端口。要完成整个操作需要两个步骤，首先在Dockerfile使用EXPOSE设置需要映射的容器端口，然后在运行容器的时候指定-p选项加上EXPOSE设置的端口，这样EXPOSE设置的端口号会被随机映射成宿主机器中的一个端口号。也可以指定需要映射到宿主机器的那个端口，这时要确保宿主机器上的端口号没有被使用。EXPOSE指令可以一次设置多个端口号，相应的运行容器的时候，可以配套的多次使用-p选项

		# 映射一个端口  
		EXPOSE port1  
		# 相应的运行容器使用的命令  
		docker run -p port1 image  
		  
		# 映射多个端口  
		EXPOSE port1 port2 port3  
		# 相应的运行容器使用的命令  
		docker run -p port1 -p port2 -p port3 image  
		# 还可以指定需要映射到宿主机器上的某个端口号  
		docker run -p host_port1:port1 -p host_port2:port2 -p host_port3:port3 image  


**ENV 设置环境变量**

* 构建指令，在image中设置一个环境变量。

		ENV <key> <value> 


**ADD  从src复制文件到container的dest路径**

* 构建指令，所有拷贝到container中的文件和文件夹权限为0755，uid和gid为0；如果是一个目录，那么会将该目录下的所有文件添加到container中，不包括目录；如果文件是可识别的压缩格式，则docker会帮忙解压缩（注意压缩格式）；如果<src>是文件且<dest>中不使用斜杠结束，则会将<dest>视为文件，<src>的内容会写入<dest>；如果<src>是文件且<dest>中使用斜杠结束，则会<src>文件拷贝到<dest>目录下。

		ADD <src> <dest>  

<src> 是相对被构建的源目录的相对路径，可以是文件或目录的路径，也可以是一个远程的文件url;
<dest> 是container中的绝对路径

**VOLUME 指定挂载点**

* 设置指令，使容器中的一个目录具有持久化存储数据的功能，该目录可以被容器本身使用，也可以共享给其他容器使用。我们知道容器使用的是AUFS，这种文件系统不能持久化数据，当容器关闭后，所有的更改都会丢失。当容器中的应用有持久化数据的需求时可以在Dockerfile中使用该指令。

**WORKDIR 切换目录**

* 设置指令，可以多次切换(相当于cd命令)，对RUN,CMD,ENTRYPOINT生效。

**ONBUILD 在子镜像中执行**
	
	ONBUILD <Dockerfile关键字>  


