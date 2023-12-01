一. 获取镜像
-------

#### 1.1. docker pull

　　镜像是运行容器的前提，也就是说没有镜像就没有办法创建容器

　　获取镜像的命令:  
　　docker pull  
　　这个命令可以直接在 docker Hub 镜像源下载镜像

　　该命令的格式是:  
　　docker pull NAME[:TAG]  
　　其中 name 是镜像仓库的名称 (用来区分镜像)tag 是镜像的标签 (用来标注版本)，通常情况下我们描述一个镜像需要包括 "名称 + 标签" 信息

```bash
我们pull一个centos的镜像
[root@rsync131 ~]# docker pull centos
Using default tag: latest
latest: Pulling from library/centos
256b176beaff: Downloading [==========>                                        ]  15.75MB/74.69MB
 
这里我没有指定标签，它的下载的默认标签是latest

```

　　下载镜像的过程解析：

　　下载过程我们可以看出，镜像文件一般由若干层组成 256b176beaff 这样的串是这个镜像的唯一 ID(这里只是一个短的 ID，实际上完整的 ID 包括 256 比特，由 64 个十六进制字符组成)。使用 docker pull 命令下载时会获取并输出镜像的各层信息。当不同的镜像包括相同的层时，本地仅存储层的一份内容，减小的存储需要的空间。 

```bash
pull 子命令支持的选项主要包括：
-a,--all-tags=true|false：是否获取仓库中的所有镜像，默认是否。

```



二. 查看镜像信息
---------

#### 2.1. docker images

　　使用 docker images 命令可以列出本地主机上的已有镜像基本信息

```bash
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
centos              latest              5182e96772bf        6 weeks ago         200MB
 
REPOSITORY：来自哪个仓库
TAG：镜像的标签
IMAGE ID：镜像的唯一ID
CREATED：创建的时间
SIZE：镜像的大小
 
 
其中镜像的ID信息是十分重要的，因为它是镜像的唯一值

```

```bash
images子命令主要支持的选项如下：
-a,--all=true|false：列出所有的镜像文件(包括临时文件)，默认为否
--digests=true|false：列出镜像的数字摘要值，默认为否
-f,--filter=[]：过滤列出的镜像，如dangling=true只显示没有被使用的镜像
--format="TEMPLATE"：控制输出格式，如.ID代表ID信息
--no-trunc=true|false：对输出格式中太长的部分是否进行截断，默认为是
-q,--quiet=true|false：仅输出ID信息，默认为否
 
更多的子命令可以通过man docker-images来查看

```

#### 2.2. docker tag

　使用 docker tag 可以给镜像添加标签信息

```bash
[root@rsync131 ~]# docker tag centos:latest mycentos:1.0
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
centos              latest              5182e96772bf        6 weeks ago         200MB
mycentos            1.0                 5182e96772bf        6 weeks ago         200MB
 
我们可以看到mycentos和centos的ID、大小完全一致，实际上他们就是一个镜像文件，只是个别名而已，docker tag 就相当于Linux创建的链接文件

```

#### 2.3. docker inspect

　　使用 docker inspect 命令可以查看镜像的详细信息，包括制作者、适应架构、各层的数字摘要等很多信息

```bash
[root@rsync131 ~]# docker inspect centos
[
    {
        "Id": "sha256:5182e96772bf11f4b912658e265dfe0db8bd314475443b6434ea708784192892",
        "RepoTags": [
            "centos:latest",
            "mycentos:1.0"
        ],
        "RepoDigests": [
            "centos@sha256:6f6d986d425aeabdc3a02cb61c02abb2e78e57357e92417d6d58332856024faf"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2018-08-06T19:21:48.235227329Z",
        "Container": "d60ffc9ddd12462af4bdcdbe45b74f3b3f99b46607ada80c3ed877b7def84250",
        "ContainerConfig": {
            "Hostname": "d60ffc9ddd12",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"/bin/bash\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:748eacc0f236df2fc9ba87c4d76a66cb10742120387e99e2acdb9454915c841d",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20180804",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "DockerVersion": "17.06.2-ce",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:748eacc0f236df2fc9ba87c4d76a66cb10742120387e99e2acdb9454915c841d",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20180804",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 199723824,
        "VirtualSize": 199723824,
        "GraphDriver": {
            "Data": {
                "MergedDir": "/var/lib/docker/overlay2/8d76bee4ae910273382e69f526ede7bbab349f6d0f04e6dc81a46c13f9f0fa92/merged",
                "UpperDir": "/var/lib/docker/overlay2/8d76bee4ae910273382e69f526ede7bbab349f6d0f04e6dc81a46c13f9f0fa92/diff",
                "WorkDir": "/var/lib/docker/overlay2/8d76bee4ae910273382e69f526ede7bbab349f6d0f04e6dc81a46c13f9f0fa92/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:1d31b5806ba40b5f67bde96f18a181668348934a44c9253b420d5f04cfb4e37a"
            ]
        },
        "Metadata": {
            "LastTagTime": "2018-09-22T12:25:06.648773262+08:00"
        }
    }
]

```

　　其实返回的就是一个 json 格式的信息，我们也可以用 -f 参数来获取指定的信息

```bash
[root@rsync131 ~]# docker inspect -f {{".Architecture"}} centos
amd64

```
[回到顶部](#Top)

#### 2.4. docker history

　　使用 docker history 命令可以查看镜像的历史信息

　　既然镜像文件是由多层组成的，那么怎么知道每一层都干了什么呢，可以使用 history 命令来查看

```bash
[root@rsync131 ~]# docker history ubuntu
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
cd6d8154f1e1        2 weeks ago         /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
 2 weeks ago         /bin/sh -c mkdir -p /run/systemd && echo 'do…   7B
 2 weeks ago         /bin/sh -c sed -i 's/^#\s*\(deb.*universe\)$…   2.76kB
 2 weeks ago         /bin/sh -c rm -rf /var/lib/apt/lists/*          0B
 2 weeks ago         /bin/sh -c set -xe   && echo '#!/bin/sh' > /…   745B
 2 weeks ago         /bin/sh -c #(nop) ADD file:3df374a69ce696c21…   84.1MB
 
 
这里有一些长的命令被自动截断了，可以使用--no-trunc选项来显示完整的信息，太长了还是自己试一试吧 没办法把输出粘贴进来 
```



三. 搜索镜像
-------

#### 3.1. docker search

　　使用 docker search 命令可以搜索远端仓库的共享镜像，默认是搜索官方仓库中的镜像文件

```bash
语法格式：
 
docker search TERM
 
支持的选项包括：
 
--automated=true|false：仅显示自动创建的镜像，默认为否
 
--no-trunc=true|false：输出信息不截断显示，默认为否
 
-s,--stars=X：指定仅显示评论为指定星级以上的镜像，默认为0，即输出所有的进镜像

```

　　搜索所有自动创建评价为 1 + 的带 nginx 的镜像

```bash
[root@rsync131 ~]# docker search --automated -s 3 nginx
Flag --automated has been deprecated, use --filter=is-automated=true instead
Flag --stars has been deprecated, use --filter=stars=3 instead
NAME                                                   DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
jwilder/nginx-proxy                                    Automated Nginx reverse proxy for docker con…   1416                                    [OK]
richarvey/nginx-php-fpm                                Container running Nginx + PHP-FPM capable of…   615                                     [OK]
jrcs/letsencrypt-nginx-proxy-companion                 LetsEncrypt container to use with nginx as p…   414                                     [OK]
webdevops/php-nginx                                    Nginx with PHP-FPM                              113                                     [OK]
zabbix/zabbix-web-nginx-mysql                          Zabbix frontend based on Nginx web-server wi…   68                                      [OK]
bitnami/nginx                                          Bitnami nginx Docker Image                      58                                      [OK]
1and1internet/ubuntu-16-nginx-php-phpmyadmin-mysql-5   ubuntu-16-nginx-php-phpmyadmin-mysql-5          44                                      [OK]
tobi312/rpi-nginx                                      NGINX on Raspberry Pi / armhf                   20                                      [OK]
blacklabelops/nginx                                    Dockerized Nginx Reverse Proxy Server.          12                                      [OK]
wodby/drupal-nginx                                     Nginx for Drupal container image                10                                      [OK]
nginxdemos/hello                                       NGINX webserver that serves a simple page co…   8                                       [OK]
webdevops/nginx                                        Nginx container                                 8                                       [OK]
1science/nginx                                         Nginx Docker images that include Consul Temp…   4                                       [OK]
behance/docker-nginx                                   Provides base OS, patches and stable nginx f…   3                                       [OK]

```



四. 删除镜像
-------

#### 4.1. docker rmi

　　使用 docker rmi 命令可以删除指定的镜像文件

```bash
语法格式：
docker rmi IMAGE [IMAGE....]
 
PS：当本地一个镜像有多个标签的时候，docker rmi 只会删除指定的标签镜像而已
 
支持的选项参数：
-f ：强制删除镜像文件

```

　　通过标签删除

```bash
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
mycentos            1.0                 5182e96772bf        6 weeks ago         200MB
centos              latest              5182e96772bf        6 weeks ago         200MB
[root@rsync131 ~]# docker rmi mycentos:1.0
Untagged: mycentos:1.0
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
centos              latest              5182e96772bf        6 weeks ago         200MB

```

　　通过 ID 删除

```bash
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
centos              latest              5182e96772bf        6 weeks ago         200MB
[root@rsync131 ~]# docker rmi 5182e96772bf
Untagged: centos:latest
Untagged: centos@sha256:6f6d986d425aeabdc3a02cb61c02abb2e78e57357e92417d6d58332856024faf
Deleted: sha256:5182e96772bf11f4b912658e265dfe0db8bd314475443b6434ea708784192892
Deleted: sha256:1d31b5806ba40b5f67bde96f18a181668348934a44c9253b420d5f04cfb4e37a
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB

```

　　强制删除

```bash
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
centos              latest              5182e96772bf        6 weeks ago         200MB
[root@rsync131 ~]# docker rmi -f 5182e96772bf
Untagged: centos:latest
Untagged: centos@sha256:6f6d986d425aeabdc3a02cb61c02abb2e78e57357e92417d6d58332856024faf
Deleted: sha256:5182e96772bf11f4b912658e265dfe0db8bd314475443b6434ea708784192892
Deleted: sha256:1d31b5806ba40b5f67bde96f18a181668348934a44c9253b420d5f04cfb4e37a
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB

```



五. 创建镜像
-------

　　其实创建镜像有三种方式，基于已有的镜像的容器创建，基于本地模板导入，基于 Dockerfile 创建，这里只介绍下前两种，因为 dockerfile 是一个很高级的东西会有专门的文章来介绍：点击 Dockerfile

#### 5.1. docker commit

　　docker commit 是基于已有的镜像的容器创建

```bash
语法格式：
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
 
支持的选项包括：
-a,--author=""：作者的信息
-c,--change=[]：提交的时候执行的Dockerfile指令，包括CMD|ENTRYPOINT|ENV|EXPOSE|LABEL|ONBUILD|USER|VOLUME|WORKDIR等
-m,--message=""：提交的信息
-p,--pause=true：提交时暂停容器运行

```

　　因为是基于容器创建的镜像，所以要先创建个容器

```bash
# 创建个容器，touch个文件，这里相对源镜像已经发生了改变 c86d860188f3 就是容器ID
[root@rsync131 ~]# docker run -it centos bash
[root@c86d860188f3 /]# touch a.txt
[root@c86d860188f3 /]# exit
exit
 
# 创建镜像基于c86d860188f3 容器
[root@rsync131 ~]# docker commit -m "add a.txt" -a "zhujingzhi" c86d860188f3 a_centos:1.0
sha256:64b8d4f3a1901353deeb31e8b646c0f04dd58accddc951814fdce237e2097a6f
 
# 查看镜像
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
a_centos            1.0                 64b8d4f3a190        19 seconds ago      200MB
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
centos              latest              5182e96772bf        6 weeks ago         200MB

```

#### 5.2. docker import

　　也可以直接从一个操作系统文件导入一个镜像，只要使用 docker import 命令

```bash
语法格式：
 
docker import [OPTIONS] file|URL|-[REPOSITORY[:TAG]]

```

　　给朋友们个链接下载模板：[https://download.openvz.org/template/precreated/](https://download.openvz.org/template/precreated/)

```bash
[root@rsync131 ~]# ll
总用量 345664
-rw-------. 1 root root      1513 8月  20 20:25 anaconda-ks.cfg
-rw-r--r--  1 root root 145639219 9月  22 14:12 centos-7-x86_64-minimal.tar.gz
 
[root@rsync131 ~]# cat centos-7-x86_64-minimal.tar.gz | docker import - centos-mini:1.0
sha256:4dec53e6e02af646126879ac69c8bee4ef6bdcf9fe0cc86d3a92959ce2e0b9cd
 
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos-mini         1.0                 4dec53e6e02a        4 seconds ago       435MB
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
centos              latest              5182e96772bf        6 weeks ago         200MB

```



六. 镜像的导出和导入
-----------

#### 6.1. docker save

　　使用 docker save 可以把镜像导出

```bash
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
a_centos            1.0                 64b8d4f3a190        17 minutes ago      200MB
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
centos              latest              5182e96772bf        6 weeks ago         200MB
[root@rsync131 ~]# docker save -o centos_bak.tar.gz centos
[root@rsync131 ~]# ll
总用量 203436
-rw-------. 1 root root      1513 8月  20 20:25 anaconda-ks.cfg
-rw-------  1 root root 208301056 9月  22 13:47 centos_bak.tar.gz

```

#### 6.2. docker load

　　使用 docker load 可以把使用 docker save 导出的文件导入

```bash
[root@rsync131 ~]# docker load --input centos_bak.tar.gz
或者
[root@rsync131 ~]# docker load < centos_bak.tar.gz
1d31b5806ba4: Loading layer [==================================================>]  208.3MB/208.3MB
Loaded image: centos:latest
[root@rsync131 ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              cd6d8154f1e1        2 weeks ago         84.1MB
centos              latest              5182e96772bf        6 weeks ago         200MB

```