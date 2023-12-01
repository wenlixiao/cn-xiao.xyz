



一、什么是 dockerfile
----------------

　　Dockerfile 是一个文本格式的配置文件，用户可以使用 Dockerfile 自定义快速创建属于自己的镜像，Dockerfile 是通过很多的参数指令编写的文件，通过 docker build 命令来创建镜像



二、基本语法和结构
---------

　　Dockerfile 由一行行的命令语句组成，并且支持以 #号注释

　　一般情况，Dockerfile 分为四部分：

1.  基础镜像信息
2.  维护者信息
3.  镜像操作指令
4.  容器启动执行的指令

　　其中，一开始必须指定所基于的镜像信息名称，接下来一般是说明的维护者信息，后面则是镜像的操作指令，例如 RUN 指令，RUN 指令将对镜像执行跟随的命令，每运行一条 RUN 指令，镜像就会添加新的一层，并提交，最后是 CMD 指令，用来指定运行容器时操作的指令



三、参数指令说明
--------


<table border="0">
  <tbody>
    <tr>
      <td>指令</td>
      <td>说明</td>
    </tr>
    <tr>
      <td>FROM</td>
      <td>指定创建镜像的基础镜像</td>
    </tr>
    <tr>
      <td>MAINTAINER</td>
      <td>指定维护者信息</td>
    </tr>
    <tr>
      <td>RUN</td>
      <td>运行命令</td>
    </tr>
    <tr>
      <td>CMD</td>
      <td>指定启动容器时默认执行的命令</td>
    </tr>
    <tr>
      <td>LABEL</td>
      <td>指定生成镜像的元数据标签信息</td>
    </tr>
    <tr>
      <td>EXPOSE</td>
      <td>声明镜像内服务所监听的端口</td>
    </tr>
    <tr>
      <td>ENV</td>
      <td>指定环境变量</td>
    </tr>
    <tr>
      <td>ADD</td>
      <td>
        复制指定的 &lt;src&gt; 路径下的内容到容器中的 &lt; dest &gt; 下，&lt;src
        &gt; 可以为 URL，如果是 tar 文件，会自动解压到 &lt; dest &gt; 路径下
      </td>
    </tr>
    <tr>
      <td>COPY</td>
      <td>
        复制本地主机的 &lt;src&gt; 路径下的内容到镜像中的 &lt; dest &gt;
        路径下，一般情况下这个常用
      </td>
    </tr>
    <tr>
      <td>ENTRYPOINT</td>
      <td>指定镜像的默认入口</td>
    </tr>
    <tr>
      <td>VOLUME</td>
      <td>创建数据卷挂载点</td>
    </tr>
    <tr>
      <td>USER</td>
      <td>指定运行容器的用户名或者 UID</td>
    </tr>
    <tr>
      <td>WORKDIR</td>
      <td>配置工作目录</td>
    </tr>
    <tr>
      <td>ARG</td>
      <td>指定镜像内使用的参数 (例如版本号等信息)</td>
    </tr>
    <tr>
      <td>ONBUILD</td>
      <td>配置当所创建的镜像作为其他镜像的基础镜像时，所执行的创建操作命令</td>
    </tr>
    <tr>
      <td>STOPSIGNAL</td>
      <td>容器退出的信号值</td>
    </tr>
    <tr>
      <td>HEALTHCHECK</td>
      <td>如何进行健康检查</td>
    </tr>
    <tr>
      <td>SHELL</td>
      <td>指定使用 shell 时默认 shell 类型</td>
    </tr>
  </tbody>
</table>


#### 3.1. FROM (小写 from)

```bash
指定所创建的镜像的基础镜像，如果不存在，会去DockerHub去下载
 
格式：
FROM
或者
FROM: 或者
FROM@ 任何Dockerfile中的第一条指令必须为FROM指令，并且如果在同一个Dockerfile中创建多个镜像，可以使用多个FROM指令(每个镜像各一条) 
```

#### 3.2. MAINTAINER (小写 maintainer)

```bash
指定维护者信息
 
格式：
MAINTAINER 该信息会写入到生成镜像的Author属性域中 
```

　　例如：

```bash
MAINTAINER zhujingzhi@126.com

```

#### 3.3. RUN (小写 run)

```bash
运行指令命令
 
格式：
RUN
或者
RUN ["executable","paraml","param2"]
注意后一个指令会被解析为JSON的数组，因此必须要用双引号
 
前者默认将在shell终端中运行命令，即/bin/sh -c
后者则是用exec执行，不会启动shell环境
 
指定使用其他的终端类型可以用第二种方式，例如 RUN ["/bin/sh","-c","echo aaaa"]
 
每条RUN指令将在当前镜像的基础上执行指定命令，并提交为新的镜像，命令长的时候可以使用 \ 来换行

```

　　例如：

```bash
RUN yum update \
    && yum -y install net-tools openssh openssl \
    && rm -rf /var/log/a.log

```

#### 3.4. CMD (小写 cmd)

```bash
CMD 指令用来指定启动容器时默认执行的命令，有三种格式：
1、CMD ["executable","param1","param2"] 使用exec执行，推荐使用
 
2、CMD command param1 parma2 在/bin/sh中执行，提供给要交互的应用
 
3、CMD ["parma1","parma2"] 提供给ENTRYPOINT 的默认参数
 
每个Dockerfile只有一条CMD命令，如果指定了多条命令，只有最后一条会执行，如果启动容器时手动指定了运行的命令(作为run的参数)，会把CMD命令给覆盖

```

#### 3.5. LABEL (小写 label)

```bash
LABEL 指令是用来指定生成镜像的元数据标签
 
格式：
LABEL = = = ....... 
```

　　例如：

```bash
LABEL version="1.0"
LABEL description="Zhu Jingzhi's mirror image"

```

#### 3.6. EXPOSE (小写 expose)

```bash
EXPOSE 声明镜像内服务监听的端口
 
格式:
EXPOSE [......]
 
注意，该指定是声明的作用，不会自动的完成端口的映射
在启动容器的时候需要使用-P 或 -p 来自动分配一个临时端口或者指定具体的端口来做映射 
```

　　例如：

```bash
EXPOSE 22 443 80

```

#### 3.7. ENV (小写 env)

```bash
ENV 指定环境变量，在镜像生成的过程中会被后续的RUN 使用，在镜像启动的容器中也会存在
 
格式：
ENV 或者
ENV=.......
 
注意在 指令指定的环境变量在运行时可以被覆盖掉
如：
docker run --env = centos 
```

　　例如：

```bash
ENV PY_VERSION 3.6.1
RUN curl -sSL http://python.org/ftp/python/3.6.1/Python-$PY_VERSION.tar.xz | tar -xJC /usr/src/python
ENV PATH /usr/src/python=$PY_VERSION/bin:$PATH

```

#### 3.8. ADD (小写 add)

```bash
该命令将复制指定的路径下的内容到容器中的路径下
 
格式:
ADD 其中可以是Dockerfile所在目录的一个相对路径(文件或者目录),也可以是URL，还可以是个tar文件(如果是tar文件，会自动解压到路径下)，可以是镜像内的绝对路径，或者相对于工作目录的相对路径
 
路径支持正则格式
例如:
ADD *.tar /code/
 
tar压缩包用这个还是很方便的 
```

#### 3.9. COPY (小写 copy)

```bash
COPY 复制本地主机的(为Dockerfile所在目录的相对路径、文件或者目录)下的内容到镜像中的目录下，目录不存在会自动创建
 
格式：
COPY 路径同样支持正则
当使用本地目录为源目录的时候，非常推荐使用户CMD 
```

　　例如：

```bash
COPY /opt/data/ /opt/

```

#### 3.10. ENTRYPOINT (小写 entrypoint)

```bash
ENTRYPOINT 指定镜像的默认入口，该入口命令会在启动容器时作为根命令执行，所有传入值作为该命令的参数
 
格式：
ENTRYPOINT ["executable","param1","param2"]
或者
ENTRYPOINT command param1 param2
 
此时，CMD指令指定的值将作为根命令的参数
每个Dockerfile 中只能有一个ENTRYPOINT 当指定多个的时候，只有最后一个生效
 
在运行时，可以被--entrypoint参数覆盖

```

#### 3.11. VOLUME (小写 volume)

```bash
VOLUME 创建一个挂载点
 
格式：
VOLUME ["/data"]
 
可以从本地主机或者其他容器挂载数据卷，一般用来存放数据库和需要保存的数据

```

#### 3.12. USER (小写 user)

```bash
USER 指定运行容器时的用户名或UID ，后续的RUN指令也是使用指定的用户
 
格式：
USER daemon
 
当服务不需要管理员权限的时候，可以使用该命令指定运行用户，并且可以在之前创建所需要的用户
 
要临时获取管理员权限可以是用sudo

```

　　例如：

```bash
RUN groupadd -r nginx && useradd -r -g nginx nginx

```

#### 3.13. WORKDIR (小写 workdir)

```bash
WORKDIR 为后续的RUN CMD ENTRYPOINT 指令配置工作目录
 
格式：
WORKDIR /path/to/workdir
 
可以使用多个WORKDIR指令，后续命令如果参数是相对路径，则会基于之前的命令指定路径

```

　　例如：

```bash
WORKDIR /a
WORKDIR b
WORKDIR c
 
最终的路径是 /a/b/c

```

#### 3.14. ARG (小写 arg)

```bash
ARG 指定一些镜像内使用的参数(例如版本信息)
 
格式：
ARG=[=]
 
也可以用docker build --build-arg=来进行指定参数值 
```

#### 3.15. ONBUILD (小写 onbuild)

```bash
ONBUILD 配置当所创建的镜像作为其他镜像的基础镜像时，所执行的创建操作指令
 
格式：
ONBUILD [INSTRYCTION]

```

　　例如创建一个镜像 A：

```bash
[......]
ONBUILD ADD ./app/src
ONBUILD RUN /usr/local/bin/python-build --dir /app/src
[......]

```

　　如果基于镜像 A 创建新的镜像，新的 Dockerfile 中使用 FROM 镜像 A 指定基础镜像，会自动执行镜像 A 中的 ONBUILD 指令的内容，等价在后面添加了两条指令

```bash
FROM 镜像A
 
# 等价于：
ADD . /app/src
RUN /usr/local/bin/python-build --dir /app/src

```

#### 3.16. STOPSIGNAL (小写 stopsignal)

```bash
STOPSIGNAL 指定所创建的镜像启动的容器接收的退出的信号值
 
例如：
STOPSIGNAL signal

```

#### 3.17. HEALTHCHECK (小写 healthcheck)

```bash
HEALTHCHECK 配置所启动容器如何进行健康检查，Docker1.12 才开始支持
 
两种格式：
1、HEALTHCHECK [OPTIONS] CMD command
# 根据所执行命令的返回值是否为0来判断
 
OPTIONS支持的参数：
--interval=DURATION(默认30s) ：过多久检查一次
--timeout=DURATION(默认30s) ： 每次检查的超时时间
--retries=N(默认为：3) ： 如果失败了重试的次数
 
2、HEALTHCHECK NONE
# 禁止基础镜像的中的健康检查

```

#### 3.18. SHELL (小写 shell)

```bash
SHELL 指定其他命令使用shell时默认shell类型
 
格式：
SHELL ["executable","parameters"]
 
默认值为["/bin/sh","-c"]

```



四、创建镜像
------

　　创建完 dockerfile 文件后 可以使用 docker build 命令来创建镜像

　　基本的格式：

　　docker build [选项] Dockerfile 路径

　　该命令会读取指定路径下 (包括子目录) 的 Dockerfile，并将该路径下的所有的内容发给 docker 服务端，由服务端来创建镜像，因此建议除非生成镜像需要，否则一般吧 Dockerfile 放到一个空的目录中

　　两点经验：

1.  如果使用非内容路径下的 Dockerfile，可以通过 -f 参数来指定路径
2.  要指定生成镜像的标签信息，可以使用 -t 参数

　　例如：

```bash
指定Dockerfile所在的路径为/opt/docker_builder,并且希望生成镜像的标签为build_repo/image
 
[root@nfs133 ~]# docker build -t zhujingzhi/nginx1.8.1 /root/docker_builder/
 
如果是在Dockerfile的目录下执行就是
 
[root@nfs133 ~]# docker build -t zhujingzhi/nginx1.8.1 .   # 一定要加.呦

```



五、Dockerfile 实战文件
-----------------

　　说了一堆的每个参数的语法格式，下面来做个实战的 dockerfile 文件，来生成一个镜像，并使用这个镜像创建个容器，并运行起来，我们来使用 nginx 服务来做实战

#### 5.1. 下载基础镜像

```bash
[root@rsync131 ~]# docker pull centos
Using default tag: latest
Trying to pull repository docker.io/library/centos ...
latest: Pulling from docker.io/library/centos
256b176beaff: Pull complete
Digest: sha256:6f6d986d425aeabdc3a02cb61c02abb2e78e57357e92417d6d58332856024faf
Status: Downloaded newer image for docker.io/centos:latest

```

#### 5.2. 编写 Dockerfile 文件

```bash
# 创建存储Dockerfile文件的目录
[root@rsync131 ~]# mkdir docker_builder
[root@rsync131 ~]# cd /root/docker_builder/
# 下载需要的包
[root@rsync131 docker_builder]# wget http://nginx.org/download/nginx-1.8.1.tar.gz
[root@rsync131 docker_builder]# wget http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm

```

　　编写 Dockerfile 文件

```bash
[root@rsync131 ~]# cd /root/docker_builder/
[root@rsync131 docker_builder]# vim Dockerfile
 
# This my first nginx Dockerfile
# Version 1.0
 
# Base images 基础镜像
FROM centos
 
#MAINTAINER 维护者信息
MAINTAINER zhujingzhi
 
#ENV 设置环境变量
ENV PATH /usr/local/nginx/sbin:$PATH
 
#ADD  文件放在当前目录下，拷过去会自动解压
ADD nginx-1.8.1.tar.gz /usr/local/
ADD epel-release-7-11.noarch.rpm /usr/local/
 
 
#RUN 执行以下命令
RUN rpm -ivh /usr/local/epel-release-7-11.noarch.rpm
RUN yum install -y wget lftp gcc gcc-c++ make openssl-devel pcre-devel pcre && yum clean all
RUN useradd -s /sbin/nologin -M nginx
 
#WORKDIR 相当于cd
WORKDIR /usr/local/nginx-1.8.1
 
RUN ./configure --prefix=/usr/local/nginx --user=nginx --group=nginx --with-http_ssl_module --with-pcre && make && make install
 
RUN ln -s /usr/local/nginx/sbin/* /usr/local/sbin/
 
#EXPOSE 映射端口
EXPOSE 80
 
#CMD 运行以下命令
CMD ["nginx","-g","daemon off;"]

```

#### 5.3. 构建镜像并启动容器

　　构建镜像

```bash
[root@rsync131 docker_builder]# cd /root/docker_builder/
[root@rsync131 docker_builder]# docker build -t zhujingzhi/nginx1.8.1 .
 
过程就不粘贴了 因为太多了 大家执行自己看一下吧，会有Dockerfile的每一步的执行详细信息

```

　　查看

```bash
[root@rsync131 docker_builder]# docker images
REPOSITORY              TAG                 IMAGE ID            CREATED              SIZE
zhujingzhi/nginx1.8.1   latest              236535a1cdd2        About a minute ago   350 MB
docker.io/registry      latest              2e2f252f3c88        12 days ago          33.3 MB
docker.io/centos        latest              5182e96772bf        6 weeks ago          200 MB
 
 
已经构建好了镜像

```

　　启动容器

```bash
[root@rsync131 docker_builder]# docker run -itd --name nginx1 -p 80:80 236535a1cdd2
8e23f4f849a33515c27e0bad92ff29442b7b2822be30dc235f30bf200d663f64
[root@rsync131 docker_builder]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
8e23f4f849a3        236535a1cdd2        "nginx -g 'daemon ..."   2 seconds ago       Up 2 seconds        0.0.0.0:80->80/tcp   nginx1

```

#### 5.4. 访问测试

```bash
[root@rsync131 docker_builder]# curl 127.0.0.1

Welcome to nginx!

    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }

Welcome to nginx!
=================

If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.

 

For online documentation and support please refer to
[nginx.org](http://nginx.org/).  
Commercial support is available at
[nginx.com](http://nginx.com/).

 

_Thank you for using nginx._


```