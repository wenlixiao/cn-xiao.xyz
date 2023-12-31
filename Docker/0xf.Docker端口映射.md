



一、容器端口映射
--------

#### 1.1. 外部访问容器

　　在启动容器时候，如果不指定参数，在容器外部是无法通过网络来访问容器内的服务的

　　当容器运行一些网络服务的时候，我们可以通过指定 - p 或者 - P 参数来实现能够让外部访问的效果

1.  -P(大 P) ：Docker 会随机映射一个 49000~49900 的端口到内部容器开放的网络端口
2.  -p(小 p)：  可以指定要映射的端口，并且在一个指定端口上只可以绑定一个容器

```bash
[root@rsync131 ~]# docker run -it -P -d --name web -v /opt/web/ centos
[root@rsync131 ~]# docker run -it -d -p 5000:5000 -v /opt/data/docker:/tmp/registry registry

```

#### 1.2. 映射到指定地址的指定端口

　　可以使用 IP:HostPort:ContainerPort 格式指定映射使用一个特定的地址

```bash
[root@rsync131 ~]# docker run -it -d -p 127.0.0.1:2000:2000 centos

```

　　还可以绑定 udp 端口

```bash
[root@rsync131 ~]# docker run -it -d -p 127.0.0.1:2000:2000/udp centos

```

#### 1.3. 映射到指定地址的任意端口

　　使用 IP::ContainerPort 绑定 IP 的任意端口到容器的 2000 端口，本地主机会自动的分配端口

```bash
[root@rsync131 ~]# docker run -it -d -p 127.0.0.1::2000 centos

```

#### 1.4. 查看映射端口的配置

　　使用 docker port 命令来查看当前映射的端口配置

```bash
[root@rsync131 ~]# docker port 6df20cdacf8d
5000/tcp -> 0.0.0.0:5000

```

　　容器有自己内部的 IP 和网络，可以使用 docker inspect + 容器名或者容器 ID 查看具体的信息



二、容器与容器直接实现互联
-------------

#### 2.1. 自定义容器名

　　容器的连接系统是根据容器的名字来执行的，所以要先给容器起一个好记得名字

　　在使用 docker run 创建容器的是 指定 --name 来定义容器的名字

```bash
[root@rsync131 ~]# docker run -it -P -d --name web1 -v /opt/web:/opt/ centos

```

　　可以使用 docker inspect 来看容器的名字

```bash
[root@rsync131 ~]# docker inspect -f  042d3c956715
/db8

```

#### 2.2. 容器互联

　　使用 --link 参数可以实现容器之间的安全交互

```bash
[root@rsync131 ~]# docker run -it -d -P --name link --link db1:db1 centos

```
