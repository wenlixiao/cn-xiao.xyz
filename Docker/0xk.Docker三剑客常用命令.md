



一、docker-machine
----------------

<table><thead><tr><th>命令</th><th>说明</th></tr></thead><tbody><tr><td>docker-machine create</td><td>创建一个 Docker 主机（常用<code>-d virtualbox</code>）</td></tr><tr><td>docker-machine ls</td><td>查看所有的 Docker 主机</td></tr><tr><td>docker-machine ssh</td><td>SSH 到主机上执行命令</td></tr><tr><td>docker-machine env</td><td>显示连接到某个主机需要的环境变量</td></tr><tr><td>docker-machine inspect</td><td>输出主机更多信息</td></tr><tr><td>docker-machine kill</td><td>停止某个主机</td></tr><tr><td>docker-machine restart</td><td>重启某台主机</td></tr><tr><td>docker-machine rm</td><td>删除某台主机</td></tr><tr><td>docker-machine scp</td><td>在主机之间复制文件</td></tr><tr><td>docker-machine start</td><td>启动一个主机</td></tr><tr><td>docker-machine status</td><td>查看主机状态</td></tr><tr><td>docker-machine stop</td><td>停止一个主机</td></tr></tbody></table>



二、docker-compose
----------------

<table border="0"><tbody><tr><td><strong>命令</strong></td><td><strong>说明</strong></td></tr><tr><td>docker-compose build</td><td>建立或者重建服务</td></tr><tr><td>docker-compose config</td><td>验证和查看 Compose 文件</td></tr><tr><td>docker-compose create　　</td><td>创建服务</td></tr><tr><td>docker-compose down</td><td>停止和删除容器，网络，镜像和卷</td></tr><tr><td>docker-compose events</td><td>从容器接收实时事件</td></tr><tr><td>docker-compose exec</td><td>登录正在运行的容器执行命令</td></tr><tr><td>docker-compose images</td><td>镜像列表</td></tr><tr><td>docker-compose kill</td><td>杀掉容器</td></tr><tr><td>docker-compose logs</td><td>查看容器的输出</td></tr><tr><td>docker-compose pause</td><td>暂停容器</td></tr><tr><td>docker-compose port</td><td>为端口绑定打印公共端口</td></tr><tr><td>docker-compose ps</td><td>容器列表</td></tr><tr><td>docker-compose pull</td><td>下载服务镜像</td></tr><tr><td>docker-compose push</td><td>上传服务镜像</td></tr><tr><td>docker-compose restart</td><td>重启容器</td></tr><tr><td>docker-compose rm</td><td>删除停止的容器</td></tr><tr><td>docker-compose run</td><td>运行一次性的命令</td></tr><tr><td>docker-compose scale</td><td>设置服务的容器数量</td></tr><tr><td>docker-compose start</td><td>启动服务</td></tr><tr><td>docker-compose stop</td><td>停止服务</td></tr><tr><td>docker-compose top</td><td>显示运行过程</td></tr><tr><td>docker-compose unpause</td><td>暂停服务</td></tr><tr><td>docker-compose up</td><td>创建并启动容器</td></tr></tbody></table>



三、docker swarm
--------------

<table><thead><tr><th>命令</th><th>说明</th></tr></thead><tbody><tr><td>docker swarm init</td><td>初始化集群</td></tr><tr><td>docker swarm join-token worker</td><td>查看工作节点的 token</td></tr><tr><td>docker swarm join-token manager</td><td>查看管理节点的 token</td></tr><tr><td>docker swarm join</td><td>加入集群中</td></tr></tbody></table>



四、docker node
-------------

<table><thead><tr><th>命令</th><th>说明</th></tr></thead><tbody><tr><td>docker node ls</td><td>查看所有集群节点</td></tr><tr><td>docker node rm</td><td>删除某个节点（<code>-f</code>强制删除）</td></tr><tr><td>docker node inspect</td><td>查看节点详情</td></tr><tr><td>docker node demote</td><td>节点降级，由管理节点降级为工作节点</td></tr><tr><td>docker node promote</td><td>节点升级，由工作节点升级为管理节点</td></tr><tr><td>docker node update</td><td>更新节点</td></tr><tr><td>docker node ps</td><td>查看节点中的 Task 任务</td></tr></tbody></table>



五、docker service
----------------

<table><thead><tr><th>命令</th><th>说明</th></tr></thead><tbody><tr><td>docker service create</td><td>部署服务</td></tr><tr><td>docker service inspect</td><td>查看服务详情</td></tr><tr><td>docker service logs</td><td>产看某个服务日志</td></tr><tr><td>docker service ls</td><td>查看所有服务详情</td></tr><tr><td>docker service rm</td><td>删除某个服务（<code>-f</code>强制删除）</td></tr><tr><td>docker service scale</td><td>设置某个服务个数</td></tr><tr><td>docker service update</td><td>更新某个服务</td></tr></tbody></table>



六、docker stack
--------------

<table><thead><tr><th>命令</th><th>说明</th></tr></thead><tbody><tr><td>docker stack deploy</td><td>部署新的堆栈或更新现有堆栈</td></tr><tr><td>docker stack ls</td><td>列出现有堆栈</td></tr><tr><td>docker stack ps</td><td>列出堆栈中的任务</td></tr><tr><td>docker stack rm</td><td>删除堆栈</td></tr><tr><td>docker stack services</td><td>列出堆栈中的服务</td></tr><tr><td>docker stack down</td><td>移除某个堆栈（不会删除数据）</td></tr></tbody></table>