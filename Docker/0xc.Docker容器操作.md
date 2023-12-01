一、创建容器
------

　　容器是 Docker 另一个核心的概念，简单来说，容器是镜像的一个运行实例，所不同的是，镜像是静态的只读文件，而容器带有运行时需要的可写文件层

#### 1.1. 新建容器

　　使用 docker create 命令来新建容器

```bash
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker.io/centos    latest              5182e96772bf        6 weeks ago         200 MB
[root@rsync131 ~]# docker create centos
b9dd06b5dcba561b178b1892631cd318f0babd4a9a3f067761963a6ab61fe078
[root@rsync131 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
b9dd06b5dcba        centos              "/bin/bash"         9 seconds ago       Created                                 sleepy_sammet

```

#### 1.2. 启动容器

　　使用 docker start 命令来启动容器

```bash
[root@rsync131 ~]# docker start b9dd06b5dcba
b9dd06b5dcba
[root@rsync131 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                     PORTS               NAMES
b9dd06b5dcba        centos              "/bin/bash"         About a minute ago   Exited (0) 4 seconds ago                       sleepy_sammet

```

　　PS: 不知道为什么我使用这样的方式是启动不了容器的，待解释

#### 1.3. 新建并启动容器

　　一般在创建容器的是我们不会使用上面的两步的方式来进行创建，有个更简单的方式就是在创建的时候直接启动

　　使用 docker run 命令，创建并启动容器

```bash
[root@rsync131 ~]# docker run -it -d --name test_centos centos
159a08c11fece853626516ab94b401464efd85d69c1f88706ec17a29178fee54
[root@rsync131 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         3 seconds ago       Up 3 seconds                            test_centos
 
 
docker run：死命令
-i：让容器的标准输入保持打开
-t：分配个伪终端
-d：守护状态运行
--name：指定容器名字参数
test_centos：容器名字
centos：镜像名或者镜像ID

```

　　这里我们说一下 docker run 执行的时候都干了什么

　　　　检查本地镜像是否存在，不存在则会自动去公有仓库去下载

　　　　利用镜像创建容器，并启动容器

　　　　分配一个文件系统给容器，并在只读的镜像层外面挂载一层可读写层

　　　　从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中

　　　　从网桥的地址池配置一个 IP 给容器

　　　　执行用户指定的应用程序

#### 1.4. 查看容器运行日志

　　使用命令 docker logs 来查看日志

```bash
[root@rsync131 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         21 minutes ago      Up 21 minutes                           test_centos
[root@rsync131 ~]# docker logs 159a08c11fec

```

[回到顶部](#_labelTop)

二、终止容器
------

#### 2.1. 终止容器

　　使用 docker stop 来终止正在运行的容器

```bash
语法格式：
docker stop 容器名
 
常用选项：
-t，--time[=10] : 10秒后发送终止信号

```

　　使用 docker kill 命令可以直接终止容器

　　docker kill 命令会直接发送 SIGKILL 信号来强行终止容器

```bash
[root@rsync131 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         29 minutes ago      Up 29 minutes                           test_centos
[root@rsync131 ~]# docker stop 159a08c11fec
[root@rsync131 ~]# docker kill 159a08c11fec

```

#### 2.2. 查看终止的容器

　　使用 docker ps 只是可以查看正在运行的容器

　　使用 docker ps -a 则可以看到所有的容器包括正在运行的和已经终止的容器

```bash
[root@rsync131 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         30 minutes ago      Up 30 minutes                           test_centos
[root@rsync131 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         30 minutes ago      Up 30 minutes                                  test_centos
b9dd06b5dcba        centos              "/bin/bash"         36 minutes ago      Exited (0) 9 minutes ago                       sleepy_sammet

```

#### 2.3. 启动和重启已经终止的容器

　　已经终止的容器我们可以使用 docker start 命令来启动

　　也可以使用 docker restart 命令将一个正在运行的容器先 stop 后 start

```bash
[root@rsync131 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         30 minutes ago      Up 30 minutes                                  test_centos
b9dd06b5dcba        centos              "/bin/bash"         36 minutes ago      Exited (0) 9 minutes ago                       sleepy_sammet
[root@rsync131 ~]# docker start b9dd06b5dcba
b9dd06b5dcba
[root@rsync131 ~]# docker restart 159a08c11fec
159a08c11fec

```

[回到顶部](#_labelTop)

三、进入容器
------

　　在使用 -d 选项来运行容器的时候，容器会在后台运行，用户是无法看到容器的信息的，也无法进入操作

　　这个时候如果我们要进入容器进行操作，应该怎么办呢？在 docker 中进入 docker 中的方法有很多种，比如官方的 attach 和 exec 命令，以及第三方的 nsenter 工具 (这个没有用过 最常用的还是 exec) 等

#### 3.1. attach 命令

```bash
attach命令是docker自带的命令
语法格式：
docker attach [--detach-keys[=[]]] [--no-stdin] [--sig-proxy[=true]] CONTAINER
 
支持的常用主要选项：
--detach-keys[=[]]：指定退出attach模式的快捷键序列，默认是CTRL+p CTRL-q
--no-stdin=true|false :是否关闭标准输入，默认true
--sig-proxy=true|false : 是否代理收到的系统信号给应用程序，默认是true

```

```bash
[root@rsync131 ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         41 minutes ago      Up 10 minutes                           test_centos
[root@rsync131 ~]# docker attach 159a08c11fec
[root@159a08c11fec /]#

```

PS：使用 attach 命令进入容器是很不方便的，就是这个命令只允许一个人使用容器，在开多个窗口同时用 attach 进入容器的时候，窗口执行的命令会同步，当一个窗口阻塞了，其他窗口就不能玩了，所有不用这个命令，太坑了

#### 3.2. exec 命令

　　为了解决上面的坑，docker 在 1.3.0 版本提供了 exec 这个命令解决了 attach 命令的坑

```bash
语法格式：
docker exec  [-d|--detach] [--detach-keys[=[]]] [-i|--interactive] [--privileged] [-t|--tty] [-u|--user[=USER]] CONTAINER COMMAND [ARG....]
 
主要选项参数：
-i， --interactive=true|false: 打开标准输入接收用户输入的命令，默认false
--privileged=true|false ：是否给执行命令以高权限，默认false
-t，--tty=true|false：分配伪终端 默认false
-u，--user="" ： 执行命令的用户或者ID

```

```bash
[root@rsync131 ~]# docker exec -it 159a08c11fec bash
[root@159a08c11fec /]# ls
anaconda-post.log  bin  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@159a08c11fec /]#

```

　　可以看到，一个 bash 终端打开了，在不影响容器内其他应用的前提下，用户可以很容易的与容器进行交互

PS：通过制定 -it 参数保持标准输入，并且分配个伪终端，是很推荐的方式哦

[回到顶部](#_labelTop)

四、删除容器
------

 　　使用 docker rm 命令来删除处于终止或者退出状态的容器

```bash
语法格式：
docker rm [-f|--force] [-l|--link] [-v|--volumes] CONTAINER [CONTAINER.....]
 
主要选项参数：
-f,--force=false：是否强行终止并删除运行中的容器
-l,--link=false：删除容器的链接，但是保留容器
-v,--volumes=false：删除容器挂载的数据卷
b
```

```bash
[root@rsync131 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                         PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         2 hours ago         Up About an hour                                   test_centos
b9dd06b5dcba        centos              "/bin/bash"         2 hours ago         Exited (0) About an hour ago                       sleepy_sammet
[root@rsync131 ~]# docker rm b9dd06b5dcba
b9dd06b5dcba
[root@rsync131 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         2 hours ago         Up About an hour                        test_centos

```

　　默认情况下，docker rm 命令只能删除已经终止的容器，不能删除运行中的容器，想要删除运行中的容器可以使用 -f 参数，来强制的删除

[回到顶部](#_labelTop)

五、容器的导入与导出
----------

　　在做容器迁移的时候我们就要先把容器导出，然后在导入到其他的地方

#### 5.1. 导出容器

　　使用命令 docker export 命令来导出容器

```bash
语法格式：
docker export [-o|--output[=""]] CONTAINER
 
主要选项参数：
-o,--output：指定导出的tar文件名

```

```bash
[root@rsync131 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
159a08c11fec        centos              "/bin/bash"         2 hours ago         Up About an hour                        test_centos
[root@rsync131 ~]# docker export -o centos_test.tar.gz 159a08c11fec
或者
[root@rsync131 ~]# docker export 159a08c11fec > centos_test.tar.gz
[root@rsync131 ~]# ll
总用量 273536
-rw-------. 1 root root      1513 8月  20 20:25 anaconda-ks.cfg
-rw-------  1 root root 280093696 9月  23 11:51 centos_test.tar.gz

```

#### 5.2. 导入容器

　　使用命令 docker import 导入变成镜像

```bash
语法格式：
docker import [-c|--change[=[]]] [-m|--messge[=MESSAGE]] file|URL|- [REPOSITORY[:TAG]]
 
用户可以通过-c 选项在导入的同时执行对容器进行修改的Dockerfile指令

```

```bash
[root@rsync131 ~]# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@rsync131 ~]# ll
总用量 273536
-rw-------. 1 root root      1513 8月  20 20:25 anaconda-ks.cfg
-rw-r--r--  1 root root 280093696 9月  23 11:52 centos_test.tar.gz
[root@rsync131 ~]# docker import centos_test.tar.gz centos:1.0
sha256:f54ee7febeda5d3d8af11cc4756c1e6661feb84b2af6280654ef07ddf8bdead2
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              1.0                 f54ee7febeda        5 seconds ago       271 MB
docker.io/centos    latest              5182e96772bf        6 weeks ago         200 MB

```