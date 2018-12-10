## 简介
Docker 是世界领先的软件容器平台。开发人员利用 Docker 可以消除协作编码时`在我的机器上可正常工作`的问题。运维人员利用 Docker 可以在`隔离容器`中并行运行和管理应用，获得更好的计算密度。企业利用 Docker 可以构建敏捷的软件交付管道，以更快的速度、更高的安全性和可靠的信誉为 Linux 和 Windows Server 应用发布新功能。		

Docker 属于 Linux 容器的一种封装，提供简单易用的容器使用接口。它是目前最流行的 Linux 容器解决方案。Docker 将`应用程序与该程序的依赖`，打包在一个文件里面。运行这个文件，就会生成一个虚拟容器。程序在这个虚拟容器里运行，就好像在真实的物理机上运行一样。有了 Docker，就不用担心`环境问题`。

总体来说，Docker 的接口相当简单，用户可以方便地创建和使用容器，把自己的应用放入容器。容器还可以进行版本管理、复制、分享、修改，就像管理普通的代码一样。

## 优点

**1、更快速的交付和部署**

对开发和运维（devop）人员来说，最希望的就是`一次创建或配置，可以在任意地方正常运行`。

开发者可以使用一个标准的镜像来构建一套开发容器，开发完成之后，运维人员可以直接使用这个容器来部署代码。 Docker 可以快速创建容器，快速迭代应用程序，并让整个过程全程可见，使团队中的其他成员更容易理解应用程序是如何创建和工作的。 Docker 容器很轻很快！容器的启动时间是`秒级`的，大量地节约开发、测试、部署的时间。

**2、更高效的虚拟化**

Docker 容器的运行不需要额外的 hypervisor 支持，它是内核级的虚拟化，因此可以实现更高的性能和效率。

**3、更轻松的迁移和扩展**

Docker 容器几乎可以在任意的平台上运行，包括物理机、虚拟机、公有云、私有云、个人电脑、服务器等。 这种兼容性可以让用户把一个应用程序从一个平台`直接迁移`到另外一个。

**4、更简单的管理**

使用 Docker，只需要小小的修改，就可以替代以往大量的更新工作。所有的修改都以`增量`的方式被分发和更新，从而实现`自动化`并且`高效`的管理。


## 安装

**安装一些必要的系统工具：**

	sudo yum install -y yum-utils device-mapper-persistent-data lvm2

**添加软件源信息：**

	sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

**更新 yum 缓存：**

	sudo yum makecache fast

**安装 Docker-ce：**

	sudo yum -y install docker-ce

**启动 Docker 后台服务**

	sudo systemctl start docker

**测试运行 hello-world**

	[root@iz2zeb1z ~]# docker run hello-world

## 容器使用

**docker 构建一个 web 应用程序**

	docker pull training/webapp  # 载入镜像

	docker run -d -P training/webapp python app.py

	docker run -d -p 5000:5000 training/webapp python app.py

	-d : 让容器在后台云心
	-P : 将容器内部网络端口映射到我们使用的主机上
	-p : 指定端口映射

**查看WEB应用容器**

	docker ps

**查看端口映射信息**

	docker port 容器ID/容器名称

**查看应用日志**

	docker logs -f 容器ID/容器名称
	-f: 让 docker logs 像使用 tail -f 一样来输出容器内部的标准输出。

**停止/重启/移除 WEB 应用容器**

	docker stop 容器ID/容器名称
	docker start 容器ID/容器名称
	docker rm 容器ID/容器名称


## 镜像使用

	当运行容器时，使用的镜像如果在本地中不存在，docker 就会自动从 docker 镜像仓库中下载，默认是从 Docker Hub 公共镜像源下载。

### 管理使用本地docker镜像

**镜像列表**

	[root@iz2zeb1zq3z ~]# docker images
	REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
	docker.io/fuhai/jpress   latest              c341f4347922        4 days ago          594 MB
	docker.io/mysql          5.6                 a876cc5d29e4        3 weeks ago         256 MB
	
**使用指定版本镜像运行容器**

_如果你不指定一个镜像的版本标签，例如你只使用 ubuntu，docker 将默认使用 ubuntu:latest 镜像。_

	docker run -t -i ubuntu:15.10 /bin/bash 
	docker run -t -i ubuntu:14.04 /bin/bash 
	
	-t:在新容器内指定一个伪终端或终端。
	-i:允许你对容器内的标准输入 (STDIN) 进行交互。

**预下载镜像**

_当我们在本地主机上使用一个不存在的镜像时 Docker 就会自动下载这个镜像。如果我们想预先下载这个镜像，我们可以使用 docker pull 命令来下载它。_

	docker pull ubuntu:13.10

**查找镜像**

	docker search httpd

|名称|描述|
|-----|-----|
| NAME | 镜像仓库源的名称 |
| DESCRIPTION | 镜像的描述 |
| OFFICIAL | 是否docker官方发布 |  

**创建镜像**

1. 从已经创建的容器中更新镜像，并且提交这个镜像

	`docker run -t -i ubuntu:15.10 /bin/bash`

	进行修改后提交

	`docker commit -m="has update" -a="study" 9a9d0f5443b5 study/ubuntu:v2`

	-m : 提交的描述信息
	-a : 指定镜像作者
	9a9d0f5443b5 ：容器ID
	study/ubuntu:v2 : 指定要创建的目标镜像名
	
	提交后 `docker images` 查看提交的镜像,使用新镜像启动容器

	 `docker run -t -i study/ubuntu:v2 /bin/bash`

2. 使用 Dockerfile 指令来创建一个新的镜像
```
	root@iz2zeb1zq3z6:~$ cat Dockerfile 
	FROM    centos:6.7
	MAINTAINER      Fisher "fisher@sudops.com"

	RUN     /bin/echo 'root:123456' |chpasswd
	RUN     useradd root
	RUN     /bin/echo 'root:123456' |chpasswd
	RUN     /bin/echo -e "LANG=\"en_US.UTF-8\"" >/etc/default/local
	EXPOSE  22
	EXPOSE  80
	CMD     /usr/sbin/sshd -D
```
每一个指令都会在镜像上创建一个新的层，每一个指令的前缀都必须是大写的。

第一条`FROM`，指定使用哪个镜像源

`RUN` 指令告诉docker 在镜像内执行命令，安装了什么。。。

然后，我们使用 Dockerfile 文件，通过 docker build 命令来构建一个镜像。

	docker build -t study/centos:6.7 .

	-t ：指定要创建的目标镜像名
	. ：Dockerfile 文件所在目录，可以指定Dockerfile 的绝对路径

	使用docker images查看创建的镜像，使用新镜像启动容器

	docker run -t -i study/centos:6.7  /bin/bash


## 容器生命周期管理命令

### run

**语法**

	docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

**OPTIONS说明**

	-a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
	-d: 后台运行容器，并返回容器ID；
	-i: 以交互模式运行容器，通常与 -t 同时使用；
	-p: 端口映射，格式为：主机(宿主)端口:容器端口
	-t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
	--name="nginx-lb": 为容器指定一个名称；
	--dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；
	--dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；
	-h "mars": 指定容器的hostname；
	-e username="ritchie": 设置环境变量；
	--env-file=[]: 从指定文件读入环境变量；
	--cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；
	-m :设置容器使用内存最大值；
	--net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
	--link=[]: 添加链接到另一个容器；
	--expose=[]: 开放一个端口或一组端口；

### start/stop/restart

	docker start :启动一个或多个已经被停止的容器

	docker stop :停止一个运行中的容器

	docker restart :重启容器

### kill（杀掉一个运行中的容器。）

**语法**

	docker kill [OPTIONS] CONTAINER [CONTAINER...]

**OPTIONS说明**
	
	-s :向容器发送一个信号

**实例**

	docker kill -s KILL mynginx


### rm（删除一个或多少容器）

**语法**

	docker rm [OPTIONS] CONTAINER [CONTAINER...]

**OPTIONS说明**
	
	-f :通过SIGKILL信号强制删除一个运行中的容器
	-l :移除容器间的网络连接，而非容器本身
	-v :-v 删除与容器关联的卷

**实例**

	docker rm -f db01 db02  强制删除容器db01、db02
	docker rm -l db01 	移除容器nginx01对容器db01的连接，连接名db
	docker rm -v nginx01	删除容器nginx01,并删除容器挂载的数据卷

### pause/unpause

**语法**

	docker pause :暂停容器中所有的进程。

	docker unpause :恢复容器中所有的进程。

**实例**

	docker pause db01 暂停数据库容器db01提供服务。
	docker unpause db01 恢复数据库容器db01提供服务。

### create（创建容器但不启动）
	
	docker create  --name myrunoob  nginx:latest      

### exec（在运行的容器中执行命令）

**语法**

	docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

**OPTIONS说明**
	
	-d :分离模式: 在后台运行
	-i :即使没有附加也保持STDIN 打开
	-t :分配一个伪终端

**实例**

	在容器mynginx中以交互模式执行容器内/root/runoob.sh脚本
	docker exec -it mynginx /bin/sh /root/runoob.sh

	在容器mynginx中开启一个交互模式的终端
	docker exec -i -t  mynginx /bin/bash


## 容器操作命令

### ps（列出容器）

**语法**

	docker ps [OPTIONS]

**OPTIONS说明**
	
	-a :显示所有的容器，包括未运行的。
	-f :根据条件过滤显示的内容。
	--format :指定返回值的模板文件。
	-l :显示最近创建的容器。
	-n :列出最近创建的n个容器。
	--no-trunc :不截断输出。
	-q :静默模式，只显示容器编号。
	-s :显示总的文件大小。

**实例**

	docker ps 	列出所有在运行的容器信息
	docker ps -n 5	列出最近创建的5个容器信息
	docker ps -a -q 	列出所有创建的容器ID


### inspect
	


### docker top (查看容器中运行的进程信息，支持 ps 命令参数)

**语法**

	docker top [OPTIONS] CONTAINER [ps OPTIONS]

**实例**

	查看容器mymysql的进程信息
	docker top mymysql		

	查看所有运行容器的进程信息
	for i in  `docker ps |grep Up|awk '{print $1}'`;do echo \ &&docker top $i; done



### attach（连接到正在运行中的容器）

**语法**

	docker attach [OPTIONS] CONTAINER

**实例**

	容器mynginx将访问日志指到标准输出，连接到容器查看访问信息
	docker attach --sig-proxy=false mynginx

### events（从服务器获取实时事件）

**语法**
	
	docker events [OPTIONS]


**OPTIONS说明**

	-f ：根据条件过滤事件；
	--since ：从指定的时间戳后显示所有事件;
	--until ：流水时间显示到指定的时间为止；


**实例**
	
	显示docker 2016年7月1日后的所有事件。
	docker events  --since="1467302400"

	显示docker 镜像为mysql:5.6 2016年7月1日后的相关事件。
	docker events -f "image"="mysql:5.6" --since="1467302400" 


### logs（获取容器的日志）

**语法**

	docker logs [OPTIONS] CONTAINER

**OPTIONS说明**

	-f : 跟踪日志输出
	--since :显示某个开始时间的所有日志
	-t : 显示时间戳
	--tail :仅列出最新N条容器日志


**实例**

	跟踪查看容器mynginx的日志输出。
	docker logs -f mynginx

	查看容器mynginx从2016年7月1日后的最新10条日志。
	docker logs --since="2016-07-01" --tail=10 mynginx


### wait（阻塞运行直到容器停止，然后打印出它的退出代码）


### export（将文件系统作为一个tar归档文件导出到STDOUT。）

**语法**

	docker export [OPTIONS] CONTAINER

**OPTIONS说明**

	-o :将输入内容写到文件。

**实例**
	
	将id为a404c6c174a2的容器按日期保存为tar文件。
	docker export -o mysql-`date +%Y%m%d`.tar a404c6c174a2

### port（查看端口映射）

	查看容器mynginx的端口映射情况。
	docker port mymysql

## 容器rootfs命令

### commit（从容器创建一个新的镜像。）

**语法**

	docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

**OPTIONS说明**

	-a :提交的镜像作者；
	-c :使用Dockerfile指令来创建镜像；
	-m :提交时的说明文字；
	-p :在commit时，将容器暂停。


**实例**

	将容器a404c6c174a2 保存为新的镜像,并添加提交人信息和说明信息。
	docker commit -a "study" -m "my apache" a404c6c174a2  mymysql:v1 

### cp（用于容器与主机之间的数据拷贝）

**语法**

	docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
	docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH


**OPTIONS说明**

	-L :保持源目标中的链接


**实例**

	将主机/www/study目录拷贝到容器96f7f14e99ab的/www目录下。
	docker cp /www/study 96f7f14e99ab:/www/

	将主机/www/study目录拷贝到容器96f7f14e99ab中，目录重命名为www。
	docker cp /www/study 96f7f14e99ab:/www

	将容器96f7f14e99ab的/www目录拷贝到主机的/tmp目录中。
	docker cp  96f7f14e99ab:/www /tmp/

### diff（检查容器里文件结构的更改）


## 镜像仓库

### login

**语法**
	
	docker login : 登陆到一个Docker镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 Docker Hub

	docker logout : 登出一个Docker镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 Docker Hub


**OPTIONS说明**

	-u :登陆的用户名
	-p :登陆的密码

**实例**

	docker login -u 用户名 -p 密码

### pull（从镜像仓库中拉取或者更新指定镜像）

**语法**
	
	docker pull [OPTIONS] NAME[:TAG|@DIGEST]


**OPTIONS说明**

	-a :拉取所有 tagged 镜像
	--disable-content-trust :忽略镜像的校验,默认开启


**实例**
	
	下载java镜像
	docker pull java

### push（将本地的镜像上传到镜像仓库,要先登陆到镜像仓库）

**语法**

	docker push [OPTIONS] NAME[:TAG]

**OPTIONS说明**

	--disable-content-trust :忽略镜像的校验,默认开启


**实例**

	docker push myapache:v1


### search（从Docker Hub查找镜像）

**语法**

	docker search [OPTIONS] TERM

**OPTIONS说明**

	--automated :只列出 automated build类型的镜像；
	--no-trunc :显示完整的镜像描述；
	-s :列出收藏数不小于指定值的镜像。


**实例**

	从Docker Hub查找所有镜像名包含java，并且收藏数大于10的镜像
	docker search -s 10 java


## 本地镜像管理

### images（列出本地镜像）

**语法**

	docker images [OPTIONS] [REPOSITORY[:TAG]]

**OPTIONS说明**

	-a :列出本地所有的镜像（含中间映像层，默认情况下，过滤掉中间映像层）；
	--digests :显示镜像的摘要信息；
	-f :显示满足条件的镜像；
	--format :指定返回值的模板文件；
	--no-trunc :显示完整的镜像信息；
	-q :只显示镜像ID。


**实例**

	列出本地镜像中REPOSITORY为ubuntu的镜像列表
	docker images  ubuntu


### rmi（删除本地一个或多少镜像）

**语法**
	
	docker rmi [OPTIONS] IMAGE [IMAGE...]


**OPTIONS说明**

	-f :强制删除；
	--no-prune :不移除该镜像的过程镜像，默认移除；

**实例**

	强制删除本地镜像runoob/ubuntu:v4。	
	docker rmi -f runoob/ubuntu:v4

### tag（标记本地镜像，将其归入某一仓库。）

**语法**

	docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]


**实例**

	将镜像ubuntu:15.10标记为 runoob/ubuntu:v3 镜像。
	docker tag ubuntu:15.10 runoob/ubuntu:v3
	
### build

**语法**


**OPTIONS说明**


**实例**


### history

**语法**


**OPTIONS说明**


**实例**


### save

**语法**


**OPTIONS说明**


**实例**


### import

**语法**


**OPTIONS说明**


**实例**

