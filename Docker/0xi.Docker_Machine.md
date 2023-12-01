



一、什么是 Docker Machine
--------------------

　　Docker Machine 是 Docker 官方编排项目之一，使用 go 语言编写的，使用不同引擎在多种平台上快速的安装 Docker 环境，开源地址:[https://github.com/docker/machine](https://github.com/docker/machine)。

　　Docker Machine 是一个工具，它允许你在虚拟宿主机上安装 Docker，并使用 docker-machine 命令管理这个宿主机，可以使用 Docker Machine 在本地的 MAC 或者 windows box、公司网络，数据中心或者 AWS 这样的云提供商上创建 docker。

　　使用 docker-machine 命令，可以启动、审查、停止、重启托管的 docker 也可以升级 Docker 客户端和守护程序并配置 docker 客户端和宿主机通信。

　　Docker Machine 也可以集中管理所以得 docker 主机。

![](0xi.Docker%20Machine.assets/1479220-20181010171322775-1488475773.png)



二、为什么要使用 Docker Machine
-----------------------

Docker Machine 使你能够在各种 Linux 上配置多个远程 Docker 宿主机。  
此外，Machine 允许你在较早的 Mac 或 Windows 系统上运行 Docker，如上一主题所述。  
Docker Machine 有这两个广泛的用例。

*   我有一个较旧的桌面系统，并希望在 Mac 或 Windows 上运行 Docker

![machine-mac-win](0xi.Docker%20Machine.assets/machine-mac-win.png)

如果你主要在不符合新的 Docker for Mac 和 Docker for Windows 应用程序的旧 Mac 或 Windows 笔记本电脑或台式机上工作，则需要 Docker Machine 来在本地 “运行 Docker”（即 Docker Engine）。在 Mac 或 Windows box 中使用 Docker Toolbox 安装程序安装 Docker Machine 将为 Docker Engine 配置一个本地的虚拟机，使你能够连接它、并运行 docker 命令。

*   我想在远程系统上配置 Docker 宿主机

![provision-use-case](0xi.Docker%20Machine.assets/provision-use-case.png)

Docker Engine Linux 系统上原生地运行。如果你有一个 Linux 作为你的主系统，并且想要运行 docker 命令，所有你需要做的就是下载并安装 Docker Engine 。然而，如果你想要在网络上、云中甚至本地配置多个 Docker 宿主机有一个有效的方式，你需要 Docker Machine。

无论你的主系统是 Mac、Windows 还是 Linux，你都可以在其上安装 Docker Machine，并使用 docker-machine 命令来配置和管理大量的 Docker 宿主机。它会自动创建宿主机、在其上安装 Docker Engine 、然后配置 docker 客户端。每个被管理的宿主机（“machine”）是 Docker 宿主机和配置好的客户端的结合。



三、Docker 和 Docker Machine 之间的区别
-------------------------------

当人们说 “Docker” 时，他们通常是指 Docker Engine，它是一个客户端 - 服务器应用程序，由 Docker 守护进程、一个 REST API 指定与守护进程交互的接口、和一个命令行接口（CLI）与守护进程通信（通过封装 REST API）。Docker Engine 从 CLI 中接受 docker 命令，例如 docker run <image>、docker ps 来列出正在运行的容器、docker images 来列出镜像，等等。

![engine](0xi.Docker%20Machine.assets/engine.png)

Docker Machine 是一个用于配置和管理你的宿主机（上面具有 Docker Engine 的主机）的工具。通常，你在你的本地系统上安装 Docker Machine。Docker Machine 有自己的命令行客户端 docker-machine 和 Docker Engine 客户端 docker。你可以使用 Machine 在一个或多个虚拟系统上安装 Docker Engine。

这些虚拟系统可以是本地的（就像你在 Mac 或 Windows 上使用 Machine 在 VirtualBox 中安装和运行 Docker Engine 一样）或远程的（就像你使用 Machine 在云提供商上 provision Dockerized 宿主机一样）。Dockerized 宿主机本身可以认为是，且有时就称为，被管理的 “machines”。

![machine](0xi.Docker%20Machine.assets/machine.png)



四、安装
----

　　Docker Mechine 可以在多种平台上安装使用，包括 Linux 、MacOS 已经 windows

　　Docker Mechine 安装非常的简单：GitHub 地址:[https://github.com/docker/machine/releases/](https://github.com/docker/machine/releases/) 里面有安装教程 (在写这篇文章的时候最新版本是 v0.15.0)

　　安装 Docker Mechine

```bash
[root@operation ~]# curl -L https://github.com/docker/machine/releases/download/v0.15.0/docker-machine-$(uname -s)-$(uname -m) >/tmp/docker-machine
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   617    0   617    0     0    462      0 --:--:--  0:00:01 --:--:--   463
100 26.8M  100 26.8M    0     0  1000k      0  0:00:27  0:00:27 --:--:-- 1373k
[root@operation ~]# chmod +x /tmp/docker-machine
[root@operation ~]# cp /tmp/docker-machine /usr/local/bin/docker-machine
 
# 查看版本确认是否安装成功
[root@operation ~]# docker-machine -v
docker-machine version 0.15.0, build b48dc28d
 
# 安装自动补全功能
[root@operation ~]# yum -y install bash-completion
[root@operation ~]# scripts=( docker-machine-prompt.bash docker-machine-wrapper.bash docker-machine.bash ); for i in "${scripts[@]}"; do wget https://raw.githubusercontent.com/docker/machine/v0.15.0/contrib/completion/bash/${i} -P /etc/bash_completion.d; done
 
# 添加以下
[root@operation ~]# cat ~/.bashrc
# .bashrc
 
# User specific aliases and functions
 
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
 
# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
 
source /etc/bash_completion.d/docker-machine-wrapper.bash
source /etc/bash_completion.d/docker-machine-prompt.bash
source /etc/bash_completion.d/docker-machine.bash
 
PS1='[\u@\h \W$(__docker_machine_ps1)]\$ '
 
# 使之生效
[root@operation ~]# source ~/.bashrc

```

　　到此位置 docker-machine 就安装完成了！



五、参数
----

　　支持命令

<table border="0" data-evernote-id="1515"><tbody data-evernote-id="1820"><tr data-evernote-id="1821"><td data-evernote-id="1517">命令</td><td data-evernote-id="1518">说明</td></tr><tr data-evernote-id="1822"><td data-evernote-id="1519">active</td><td data-evernote-id="1520">查看当前激活状态的 Docker 主机</td></tr><tr data-evernote-id="1823"><td data-evernote-id="1521">config</td><td data-evernote-id="1522">查看当前激活状态 Docker 主机的连接信息</td></tr><tr data-evernote-id="1824"><td data-evernote-id="1523">creat</td><td data-evernote-id="1524">&nbsp;创建 Docker 主机</td></tr><tr data-evernote-id="1825"><td data-evernote-id="1525">env</td><td data-evernote-id="1526">显示连接到某个主机需要的环境变量</td></tr><tr data-evernote-id="1826"><td data-evernote-id="1527">inspect</td><td data-evernote-id="1528">以 json 格式输出指定 Docker 的详细信息</td></tr><tr data-evernote-id="1827"><td data-evernote-id="1529">ip</td><td data-evernote-id="1530">获取指定 Docker 主机的地址</td></tr><tr data-evernote-id="1828"><td data-evernote-id="1531">kill</td><td data-evernote-id="1532">直接杀死指定的 Docker 主机</td></tr><tr data-evernote-id="1829"><td data-evernote-id="1533">ls</td><td data-evernote-id="1534">列出所有的管理主机</td></tr><tr data-evernote-id="1830"><td data-evernote-id="1535">provision</td><td data-evernote-id="1536">重新配置指定主机</td></tr><tr data-evernote-id="1831"><td data-evernote-id="1537">regenerate-certs</td><td data-evernote-id="1538">为某个主机重新生成 TLS 信息</td></tr><tr data-evernote-id="1832"><td data-evernote-id="1539">restart</td><td data-evernote-id="1540">重启指定的主机</td></tr><tr data-evernote-id="1833"><td data-evernote-id="1541">rm</td><td data-evernote-id="1542">删除某台 Docker 主机，对应的虚拟机也会被删除</td></tr><tr data-evernote-id="1834"><td data-evernote-id="1543">ssh</td><td data-evernote-id="1544">通过 SSH 连接到主机上，执行命令</td></tr><tr data-evernote-id="1835"><td data-evernote-id="1545">scp</td><td data-evernote-id="1546">在 Docker 主机之间以及 Docker 主机和本地主机之间通过 scp 远程复制数据</td></tr><tr data-evernote-id="1836"><td data-evernote-id="1547">mount</td><td data-evernote-id="1548">使用 SSHFS 从计算机装载或卸载目录</td></tr><tr data-evernote-id="1837"><td data-evernote-id="1549">start</td><td data-evernote-id="1550">启动一个指定的 Docker 主机，如果对象是个虚拟机，该虚拟机将被启动</td></tr><tr data-evernote-id="1838"><td data-evernote-id="1551">status</td><td data-evernote-id="1552">获取指定 Docker 主机的状态 (包括：Running、Paused、Saved、Stopped、Stopping、Starting、Error) 等</td></tr><tr data-evernote-id="1839"><td data-evernote-id="1553">stop</td><td data-evernote-id="1554">停止一个指定的 Docker 主机</td></tr><tr data-evernote-id="1840"><td data-evernote-id="1555">upgrade</td><td data-evernote-id="1556">将一个指定主机的 Docker 版本更新为最新</td></tr><tr data-evernote-id="1841"><td data-evernote-id="1557">url</td><td data-evernote-id="1558">获取指定 Docker 主机的监听 URL</td></tr><tr data-evernote-id="1842"><td data-evernote-id="1559">version</td><td data-evernote-id="1560">显示 Docker Machine 的版本或者主机 Docker 版本</td></tr><tr data-evernote-id="1843"><td data-evernote-id="1561">help</td><td data-evernote-id="1562">显示帮助信息</td></tr></tbody></table>

　　支持的平台及驱动引擎

```bash
# 平台
1.常规Linux操作系统；
 
2.虚拟化平台-VirtualBox，VMware，Hyper-V
 
3.Openstack
 
4.公有云-Amazon Web Services，Microsoft Azure，Google Compute Engine，Digital Ocean等
 
Docker Machine为这些环境起了一个统一的名字：provider
 
对于特定的某个provider，Docker Machine使用相应的driver安装配置docker host
 
# 驱动引擎
amazonec2
azure
digitalocean
exoscale
generic
google
hyperv
none
openstack
rackspace
softlayer
virtualbox
vmwarevcloudair
vmwarefusion
vmwarevsphere
 
# 指定方式
使用参数 -d 或者 --driver 驱动引擎名称

```

　　说白了都是虚拟化平台和云平台的驱动文件



六、使用
----

　　通过 Docker Machine 创建 docker(我用了两台机器)

*   192.168.31.43  安装 docker machine 的机器  主机名：operation
*   192.168.31.188  被管理的机器       主机名：client1

　　创建步骤：

　　1. 配置主机间的 SSH 免密 (在 192.168.31.43 上面创建)

```bash
# 生成keys并配置可以免密登录主机(这个是必须要做的)
[root@operation ~]# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:0Fq7VlowSsDqSStOn4veIoTxFbW2RB059qXMSzLblKg root@operation
The key's randomart image is:
+---[RSA 2048]----+
|   ...o..o       |
|    oo o=   .    |
|   . .*.=* +     |
|. o .+ *++O      |
|.= +  +.SBo.     |
|oo=   E .=o      |
|+.. .   +        |
|.ooo   .         |
|oo.o.            |
+----[SHA256]-----+
 
# 将keys拷贝到client1上去
[root@operation ~]# ssh-copy-id root@192.168.31.188
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
The authenticity of host '192.168.31.188 (192.168.31.188)' can't be established.
ECDSA key fingerprint is SHA256:6MKhx743bCMD3Ay+ELNpKnq1+3/wltcrPhbuyEclZj8.
ECDSA key fingerprint is MD5:e8:6d:14:7e:41:da:96:4b:2c:92:f8:61:cc:f4:7b:14.
Are you sure you want to continue connecting (yes/no)? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@192.168.31.188's password:
 
Number of key(s) added: 1
 
Now try logging into the machine, with:   "ssh 'root@192.168.31.188'"
and check to make sure that only the key(s) you wanted were added.
 
# 测试是否可以免密登录
[root@operation ~]# ssh root@192.168.31.188
Last login: Fri Oct 12 15:27:45 2018 from 192.168.31.104
[root@client1 ~]# exit

```

　　2. 使用 docker machine 创建 docker host

```bash
# 使用docker machine 创建
# 对于docker machine来将，术语Machine就是运行docker daemon的主机，创建machine就是在host上安装docker
# 执行docker-macine ls查看当前的machine
[root@operation ~]# docker-machine ls
NAME   ACTIVE   DRIVER   STATE   URL   SWARM   DOCKER   ERRORS
 
# 当前还没有一个machine，接下来我们创建第一个machine：docker188-192.168.31.188  
[root@operation ~]# docker-machine create --driver generic --generic-ip-address=192.168.31.188 docker188
Running pre-create checks...
Creating machine...
(docker188) No SSH key specified. Assuming an existing key at the default location.
Waiting for machine to be running, this may take a few minutes...
Detecting operating system of created instance...
Waiting for SSH to be available...
Detecting the provisioner...
Provisioning with centos...
Copying certs to the local machine directory...
Copying certs to the remote machine...
Setting Docker configuration on the remote daemon...
Checking connection to Docker...
Docker is up and running!
To see how to connect your Docker Client to the Docker Engine running on this virtual machine, run: docker-machine env docker188
 
注意：这里会出现Error creating machine: Error running provisioning: error installing docker:  这样的错误 原因就是因为网络的原因，没有安装docker
 
# 创建成功执行ls查看
[root@operation ~]# docker-machine  ls
NAME        ACTIVE   DRIVER    STATE     URL                         SWARM   DOCKER        ERRORS
docker188   -        generic   Running   tcp://192.168.31.188:2376           v18.06.1-ce  
 
# 登录到client查看配置项
[root@operation ~]# ssh root@192.168.31.188
Last login: Fri Oct 12 16:19:10 2018 from 192.168.31.43
[root@docker188 ~]# cat /etc/systemd/system/docker.service.d/10-machine.conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2376 -H unix:///var/run/docker.sock --storage-driver devicemapper --tlsverify --tlscacert /etc/docker/ca.pem --tlscert /etc/docker/server.pem --tlskey /etc/docker/server-key.pem --label provider=generic
Environment=
 
注：-H tcp://0.0.0.0:2376 使docker daemon接受远程连接
 
      --tls*对远程连接启用安全认证和加密
 
注：大家可能会发现这里的主机名变成了docker188 原因就是docker-machine创建的时候会把主机名也一起修改
 
 
# 查看docker188的环境变量
[root@operation ~]# docker-machine env docker188
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.31.188:2376"
export DOCKER_CERT_PATH="/root/.docker/machine/machines/docker188"
export DOCKER_MACHINE_
# Run this command to configure your shell:
# eval $(docker-machine env docker188)
 
# 根据提示执行
[root@operation ~]# eval $(docker-machine env docker188)
[root@operation ~ [docker188]]#
 
可以看到，命令提示符变成了docker188，其原因是我们之前在/root/.bashrc里面配置了 PS1='[\u@\h \W$(__docker_machine_ps1)]\$ '，用于显示当前的docker host
注：如果我们输入eval $(docker-machine env docker1)没有显示出docker188的命令提示符，我们可以重新输入一遍 PS1='[\u@\h \W$(__docker_machine_ps1)]\$ '
在此状态下执行的docker命令其效果都相当于在docker188上执行
 
[root@operation ~ [docker188]]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
[root@operation ~ [docker188]]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE

```

　　3. 其他命令操作

```bash
# 其他命令
# create 命令
选项包括：
·--driver，-d"none"                             指定驱动类型；
·--engine-install-url"https://get.docker.com"   配置Dokcer主机时候的安装URL；
·--engine-opt option                            以键值对格式指定所创建Docker引擎的参数；
·--engine-insecure-registry option              以键值对格式指定所创建Docker引擎允许访问的不支持认证的注册仓库服务；
·--engine-registry-mirror option                指定使用注册仓库镜像；
·--engine-label option                          为所创建的Docker引擎添加标签；
·--engine-storage-driver                        存储后端驱动类型；
·--engine-env option                            指定环境变量；
·--swarm                                        指定使用Swarm；
·--swarm-image"swarm：latest"                   使用Swarm时候采用的镜像；
·--swarm-master                                 配置机器作为Swarm集群的master节点；
·--swarm-discovery                              Swarm集群的服务发现机制参数；
·--swarm-strategy"spread"                       Swarm默认调度策略；
·--swarm-opt option                             任意传递给Swarm的参数；
·--swarm-host"tcp://0.0.0.0：3376"              指定地址将监听 Swarm master节点请求；
·--swarm-addr                                   从指定地址发送广播加入Swarm集群服务。
 
实例：
docker-machine create -d virtualbox \
--engine-storage-driver overlay \
--engine-label name=testmachine \
--engine-label year=2018 \
--engine-opt dns=8.8.8.8 \
--engine-env HTTP_PROXY=http://proxy.com:3128 \
--engine-insecure-registry registry.private.com \
mydockermachine
 
 
# active命令
[root@operation ~]# docker-machine ls
NAME        ACTIVE   DRIVER    STATE     URL                         SWARM   DOCKER        ERRORS
docker188   -        generic   Running   tcp://192.168.31.188:2376           v18.06.1-ce  
 
# 这里的状态是没有被激活
[root@operation ~]# docker-machine env docker188
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.31.188:2376"                           # 激活主机
export DOCKER_CERT_PATH="/root/.docker/machine/machines/docker188"
export DOCKER_MACHINE_
# Run this command to configure your shell:
# eval $(docker-machine env docker188)
 
[root@operation ~]# export DOCKER_HOST="tcp://192.168.31.188:2376"
[root@operation ~]# docker-machine ls
NAME        ACTIVE   DRIVER    STATE     URL                         SWARM   DOCKER        ERRORS
docker188   *        generic   Running   tcp://192.168.31.188:2376           v18.06.1-ce  
[root@operation ~]# docker-machine active
docker188
 
# config命令
[root@operation ~]# docker-machine config docker188
--tlsverify
--tlscacert="/root/.docker/machine/machines/docker188/ca.pem"
--tlscert="/root/.docker/machine/machines/docker188/cert.pem"
--tlskey="/root/.docker/machine/machines/docker188/key.pem"
-H=tcp://192.168.31.188:2376
 
# inspect命令
[root@operation ~]# docker-machine inspect docker188
{
    "ConfigVersion": 3,
    "Driver": {
        "IPAddress": "192.168.31.188",
        "MachineName": "docker188",
        "SSHUser": "root",
        "SSHPort": 22,
        "SSHKeyPath": "",
        "StorePath": "/root/.docker/machine",
        "SwarmMaster": false,
        "SwarmHost": "",
        "SwarmDiscovery": "",
        "EnginePort": 2376,
        "SSHKey": ""
    },
    "DriverName": "generic",
    "HostOptions": {
        "Driver": "",
        "Memory": 0,
        "Disk": 0,
        "EngineOptions": {
            "ArbitraryFlags": [],
            "Dns": null,
            "GraphDir": "",
            "Env": [],
            "Ipv6": false,
            "InsecureRegistry": [],
            "Labels": [],
            "LogLevel": "",
            "StorageDriver": "",
            "SelinuxEnabled": false,
            "TlsVerify": true,
            "RegistryMirror": [],
            "InstallURL": "https://get.docker.com"
        },
        "SwarmOptions": {
            "IsSwarm": false,
            "Address": "",
            "Discovery": "",
            "Agent": false,
            "Master": false,
            "Host": "tcp://0.0.0.0:3376",
            "Image": "swarm:latest",
            "Strategy": "spread",
            "Heartbeat": 0,
            "Overcommit": 0,
            "ArbitraryFlags": [],
            "ArbitraryJoinFlags": [],
            "Env": null,
            "IsExperimental": false
        },
        "AuthOptions": {
            "CertDir": "/root/.docker/machine/certs",
            "CaCertPath": "/root/.docker/machine/certs/ca.pem",
            "CaPrivateKeyPath": "/root/.docker/machine/certs/ca-key.pem",
            "CaCertRemotePath": "",
            "ServerCertPath": "/root/.docker/machine/machines/docker188/server.pem",
            "ServerKeyPath": "/root/.docker/machine/machines/docker188/server-key.pem",
            "ClientKeyPath": "/root/.docker/machine/certs/key.pem",
            "ServerCertRemotePath": "",
            "ServerKeyRemotePath": "",
            "ClientCertPath": "/root/.docker/machine/certs/cert.pem",
            "ServerCertSANs": [],
            "StorePath": "/root/.docker/machine/machines/docker188"
        }
    },
    "Name": "docker188"
}
 
 
# ssh命令
[root@operation ~]# docker-machine ssh docker188 docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              196d12cf6ab1        4 weeks ago         4.41MB
 
[root@operation ~]# docker-machine ssh docker188   
Last login: Fri Oct 12 16:36:49 2018 from 192.168.31.43
[root@docker188 ~]#
 
# url命令
[root@operation ~]# docker-machine url docker188                  
tcp://192.168.31.188:2376
 
# status命令
[root@operation ~]# docker-machine status docker188
Running
 
# version命令
[root@operation ~]# docker-machine version docker188
18.06.1-ce

```

　　注：还有一些命令就不一一列出了，可以查看上面的参数命令表，具体命令的使用方法可以通过 --help 查看

　　注：Machine 安装 docker 环境中会因网络或其他情况造成安装失败，使用中发现，这种安装失败频率很高，感觉没有使用的价值，说白了，一个公司操作系统一般不会超过两个发行版，写个脚本一键安装也许会更方便！



七、总结
----

Docker Machine 最主要有两个作用：

*   使用 Docker Machine 方便在不同的环境中使用 Docker ，比如：Win/Mac
*   使用 Docker Machine 方便在云环境下批量部署 Docker 环境，比如：私有云，公有云批量安装 Docker 环境



八、参考链接
------

官方地址：[https://docs.docker.com/machine/](https://docs.docker.com/machine/)

官方驱动详细使用方法：[https://docs.docker.com/machine/drivers/](https://docs.docker.com/machine/drivers/)

参考文档：[https://www.cnblogs.com/lkun/p/7781157.html](https://www.cnblogs.com/lkun/p/7781157.html)