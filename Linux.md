##目录结构
* bin
	* 存放着经常使用的命令
* boot
	* 核心文件，包括一些连接文件以及镜像文件

* dev是Device(设备)的缩写
	*  该目录下存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。
* /etc：
	* 这个目录用来存放所有的系统管理所需要的配置文件和子目录。
* /home：
	* 用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。
* /lib：
	* 这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。
* /lost+found：
	* 这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。
* /media 
	* linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。
* /mnt：
	* 系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。
* /opt：
	* 这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。
* /proc：
	* 这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器：
* echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
* /root：
	* 该目录为系统管理员，也称作超级权限者的用户主目录。
* /sbin：
	* s就是Super User的意思，这里存放的是系统管理员使用的系统管理程序。
* /selinux：
	* 这个目录是Redhat/CentOS所特有的目录，Selinux是一个安全机制，类似于windows的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。
* /srv：
	* 该目录存放一些服务启动之后需要提取的数据。
* /sys：
	* 这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统 sysfs 。
	sysfs文件系统集成了下面3种文件系统的信息：针对进程信息的proc文件系统、针对设备的devfs文件系统以及针对伪终端的devpts文件系统。
	该文件系统是内核设备树的一个直观反映。
	当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。
* /tmp：
	* 这个目录是用来存放一些临时文件的。
* /usr：
 	* 这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与windows下的program files目录。
* /usr/bin：
	* 系统用户使用的应用程序。
* /usr/sbin：
	* 超级用户使用的比较高级的管理程序和系统守护程序。
* /usr/src：内核源代码默认的放置目录。
* /var：
	* 这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

##打开系统监视器
* gnome-system-moitor


##解压

* tar -xvf file.tar //解压 tar包

* tar -xzvf file.tar.gz //解压tar.gz

* tar -xjvf file.tar.bz2   //解压 tar.bz2

* tar -xZvf file.tar.Z   //解压tar.Z

* unrar e file.rar //解压rar

* unzip file.zip //解压zip



##用户及用户组
###一、创建用户：

* 1、使用命令 useradd

例：useradd user1——创建用户user1
    useradd –e 12/30/2009 user2——创建user2,指定有效期2009-12-30到期
    用户的缺省UID从500向后顺序增加，500以下作为系统保留账号，可以指定UID，

例：useradd –u 600 user3

   

* 2、使用 passwd 命令为新建用户设置密码
例：passwd user1
注意：没有设置密码的用户不能使用。

 

* 3、命令 usermod 修改用户账户
例：将用户 user1的登录名改为  u1，
usermod –l u1 user1
例：将用户 user1 加入到 users组中，
usermod –g users user1


例：将用户 user1 目录改为/users/us1
usermod –d /users/us1 user1

 

* 4、使用命令 userdel 删除用户账户
例：删除用户user2
userdel user2
例：删除用户 user3，同时删除他的工作目录
userdel –r user3

 

* 5、查看用户信息
id命令查看一个用户的UID和GID, 例：查看user4的id
id user4
finger命令 ——可以查看用户的主目录、启动shell、用户名、地址、电话等信息
例：finger user4

 

 

### 二、用户组：

* 6、命令 groupadd创建用户组
groupadd –g 888 users
创建一个组users，其GID为888

 

* 7、命令 gpasswd为组添加用户
只有root和组管理员能够改变组的成员：
例：把 user1加入users组
gpasswd –a user1 users
例：把 user1退出users组
gpasswd –d user1 users

* 8、命令groupmod修改组
groupmod –n user users       修改组名user为users

 

* 9、groupdel删除组
groupdel users    删除组users