一、什么是 Docker
------------

　　Docker 是基于 GO 语言实现的开源容器项目，诞生于 2013 年年初，最初发起者是 dotCloud 公司，Docker 项目已经加入了 Linux 基金会，并遵循 apache2.0 协议，全部代码开源在 github 上，docker 的构想是要实现 "Build ship and run any,anywhere", 即通过对应用的封装、分发、部署、运行 生命周期进行管理，达到应用组件 "一次封装，到处运行" 的目的。

　　Docker 是一种容器虚拟化，Docker 容器可以理解为一种轻量级的沙盒，每个容器内运行着一个应用，不同的容器相互隔离，容器之间也可以通过网络互相通信，也可以说 docker 就是轻量级及互相隔离应用的虚拟化技术



二、Docker 的优势
------------

　　更快的交付和部署

　　更高效的资源利用

　　更轻松的迁移和扩展

　　更简单的更新管理

　　docker 容器很快，启动和停止可以在秒级实现

　　docker 容器对系统资源需求很少，一台主机上可以同时运行数千个容器

　　docker 通过类型 git 设计理念的操作来方便用户获取、分发、更新应用镜像，存储复用，增量更新

　　docker 利用 Linux 系统上的多种防护技术实现了严格的隔离可靠性，并且可以整合众多的安全工具



三、Docker 容器技术和传统虚拟机技术的区别
------------------------



**传统的方式是在硬件层面实现虚拟化，需要有额外的虚拟机管理应该和虚拟机操作系统层**



![image-20220721140247586](0xa.Docker%E5%AE%89%E8%A3%85.assets/image-20220721140247586-165838337393213.png)







　**docker 容器是在操作系统层面上实现的虚拟化，直接复用本地主机的操作系统，因此更加的轻量级**





![image-20220721140319843](0xa.Docker%E5%AE%89%E8%A3%85.assets/image-20220721140319843-165838340125115.png)



四、Docker 核心三大概念
---------------

　　docker 的大部分操作都是围绕着它的三大核心概念 -- 镜像、容器、仓库来展开的 这个就相当于 docker 的基础很重要

#### 4.1. docker 镜像

　　docker 镜像类似于虚拟机的镜像文件，可以将它理解为一个只读的模板，例如一个镜像可以包含一个基本的操作系统，里面安装这个应用程序镜像是创建 docker 容器的基础

　　详细信息请点击 -----> 镜像详情

#### 4.2. docker 容器

　　docker 容器就像一个轻量级的沙盒，docker 利用容器来运行和隔离应用，容器是从镜像创建的应用运行实例，可以启动、停止、删除，而这些容器都是互相隔离，互不可见的

　　详细信息请点击 -----> 容器详情

#### 4.3. docker 仓库

　　docker 仓库类型于代码仓库，它是集中存放镜像文件的地方

　　详细信息请点击 -----> 仓库详情

PS：镜像自身是只读的，容器从镜像启动的时候，会在镜像的最上层创建一个可写层



五、安装 Docker
-----------

```bash
yum -y install docker-io
或者
curl -fsSL https://get.docker.com/ | sh
 
安装指定版本的docker
在使用centos7，并使用yum安装docker的时候，往往不希望安装最新版本的docker，而是希望安装与自己熟悉或者当前业务环境需要的版本
# 安装依赖包
yum install -y yum-utils device-mapper-persistent-data lvm2 libsemanage-python libselinux-utils
# 添加Docker软件包源
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 关闭测试版本list（只显示稳定版）
yum-config-manager --enable docker-ce-edge
yum-config-manager --enable docker-ce-test
# 更新yum包索引
yum makecache fast
# 找到需要安装的
yum list docker-ce --showduplicates|sort -r 
# 指定版本安装
yum install docker-ce-17.09.0.ce -y
# 启动docker
systemctl start docker && systemctl enable docker
 
查看docker版本
docker version　

```



六、Docker 服务配置项
--------------

```bash
配置文件：/etc/default/docker
进程ID：/var/run/docker.pid
日志文件:/var/log/upstart/docker.log

```