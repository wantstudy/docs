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

### 容器

### 镜像

### 连接


## 容器生命周期管理命令

### run

### start/stop/restart

### kill

### rm

### pause/unpause

### create

### exec

## 容器操作命令

### ps

### inspect

### top

### attach

### events

### logs

### wait

### export

### port

## 容器rootfs命令

### commit

### cp

### diff

## 镜像仓库

### login

### pull

### push

### search

## 本地镜像管理

### images

### rmi

### tag

### build

### history

### save

### import
