#### 1、项目名称

nowcoder-community

#### 2、服务划分

```shell
vue-admin
vue-web

nowcoder-picture
nowcoder-sms
nowcoder-search
nowcoder-monitor

nowcoder-api
nowcoder-commons
nowcoder-base
nowcoder-xo
nowcoder-utils
```



#### 3、项目搭建

```shell
#github新建仓库

#idea code 仓库

#新增模块

#maven配置

#项目结构调整

#父pom设置

#子pom设置

#.gitignore设置
```



#### 4、Docker配置环境

**安装docker**

```shell
#进入root

#卸载
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
#建立仓库
yum install -y yum-utils
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
#安装docker引擎
 sudo yum install docker-ce docker-ce-cli containerd.io
#启动
sudo systemctl start docker
#测试
sudo docker run hello-world
#设置开机自启
systemctl enable docker.service
```



**docker常用命令**

```shell
#若已经是root，不需要sudo

#启动
sudo systemctl start docker
#停止
sudo systemctl stop docker
#重启
sudo systemctl restart docker
#配置生效
sudo systemctl daemon-reload
sudo systemctl restart docker
#版本
docker version
#信息
docker info
#帮助
docker --help
#开机自启
systemctl enable docker.service


#查看已安装镜像
docker images
#搜索镜像
docker search tomcat
#下载镜像
docker pull tomcat[:version]
#删除镜像
docker rmi tomcat[:version]
# 删除全部，docker images -qa : 获取所有镜像ID
docker rmi -f $(docker images -qa)


#启动容器
docker run [options] image(name:tag) [command] [arg...]
    -d: 后台运行容器,并返回容器ID
    -i: 以交互式运行容器,通常与-t同时使用
    -p: 端口映射,格式为 主机(宿主)端口:容器端口
    -t: 为容器重新分配一个伪输入终端,通常与-i同时使用
    --name="name": 为容器指定一个名称
    --dns 8.8.8.8: 为容器指定一个dns服务器,默认与宿主一致
    --dns-search domain:为容器指定一个DNS域名,默认与宿主一致
    -h "hostname": 指定容器的hostname
    -e arg="value": 设置环境变量
    -env-file=[]:从指定文件读入环境变量
    --cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定的cpu运行
    -m: 设置容器使用内存最大值
    --net="bridge": 指定容器的网络连接类型,支持bridge/host/none/container四种类型
    --link=[]:添加链接到另外一个容器
    --expose=[]:开放一个端口或一组端口,宿主机使用随机端口映射到开放的端口
#运行的容器
docker ps
    -a:显示所有容器，包括当前没有运行的容器
    -l:显示最近创建的容器
    -n:显示最近创建的N个容器
    -q: 静默模式,只显示容器ID
    --no-trunc不截断输出:
#退出容器
exit:退出并停止
ctrl+P+Q:容器不停止退出
#启动容器
docker start 容器ID或容器name
#重启容器
docker restart 容器ID或容器name
#停止容器
docker stop 容器ID或容器name
#杀死容器
docker kill 容器ID或容器name
# 删除已经停止的容器
docker rm 容器ID或容器name 
# 强制删除已经停止或正在运行的容器
docker rm -f  容器ID或容器name 
#一次性删除所有正在运行的容器
docker rm -f $(docker ps -qa)
#从容器拷贝文件
docker cp 容器ID或容器名称:/文件路径与文件名 宿主机地址
#进入容器
docker exec -it 容器id /bin/bash


#查看日志
docker logs -f -t --tail 10 容器ID或容器名称
    -t:加入时间戳
    -f:跟随最新的日志打印
    --tail 行数:输出最后几行的日志
```



**安装mysql**

```shell
#进入root用户
#拉取镜像
docker pull mysql:8.0
#运行并挂载
docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:8.0

#查看状态
docker ps
docker ps -a

#添加配置文件
cd /mydata/mysql/conf
vi my.cnf
    [client]
    default-character-set=utf8
    [mysql]
    default-character-set=utf8
    [mysqld]
    init_connect='SET collation_connection = utf8_unicode_ci'
    init_connect='SET NAMES utf8'
    character-set-server=utf8
    collation-server=utf8_unicode_ci
    skip-character-set-client-handshake
    skip-name-resolve
#重启
docker restart mysql
#设置其启动
docker update mysql --restart=always
```



**安装redis**

```shell
#下载镜像
docker update mysql --restart=always
#创建挂载的目录和文件
mkdir -p /mydata/redis
cd /mydata/redis
mkdir data
vi myredis.conf
protected-mode no
port 6379
tcp-backlog 511
requirepass 000415
timeout 0
tcp-keepalive 300
daemonize no
supervised no
pidfile /var/run/redis_6379.pid
loglevel notice
logfile ""
databases 30
always-show-logo yes
save 900 1
save 300 10
save 60 10000
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump.rdb
dir ./
replica-serve-stale-data yes
replica-read-only yes
repl-diskless-sync no
repl-disable-tcp-nodelay no
replica-priority 100
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no
appendonly yes
appendfilename "appendonly.aof"
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
aof-use-rdb-preamble yes
lua-time-limit 5000
slowlog-max-len 128
notify-keyspace-events ""
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-size -2
list-compress-depth 0
set-max-intset-entries 512
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
hll-sparse-max-bytes 3000
stream-node-max-bytes 4096
stream-node-max-entries 100
activerehashing yes
hz 10
dynamic-hz yes
aof-rewrite-incremental-fsync yes
rdb-save-incremental-fsync yes
#启动
docker run --restart=always \
--log-opt max-size=100m \
--log-opt max-file=2 \
-p 6379:6379 \
--name redis \
-v /mydata/redis/myredis.conf:/etc/redis/redis.conf \
-v /mydata/redis/data:/data \
-d redis:7.0.5 \
redis-server /etc/redis/redis.conf \
--appendonly yes  \
--requirepass root
```



**安装elasticsearch**

```shell
#修改系统内存
sysctl -w vm.max_map_count=262144
#下载镜像
docker pull elasticsearch:7.6.2
#创建挂载文件，赋予执行权限
mkdir /mydata/es/data
mkdir /mydata/es/plugins
chmod 777 /mydata/es/data
chmod 777 /mydata/es/plugins
#启动镜像
docker run -d \
--restart=always \
--name elasticsearch \
-v /mydata/es/data:/usr/share/elasticsearch/data \
-v /mydata/es/plugins:/usr/share/elasticsearch/plugins \
--privileged \
-e ES_JAVA_OPTS="-Xms512m -Xmx512m" \
-e "discovery.type=single-node" \
-p 9200:9200 -p 9300:9300 \
elasticsearch:7.6.2
#传输ik分析器到/mydata/plugins, 并解压到plugins/ik
unzip -d /mydata/es/plugins/ik elasticsearch-analysis-ik-7.6.2.zip
docker cp ik elasticsearch:/usr/share/elasticsearch/plugins
#重启
docker restart elasticsearch
```



**安装kafka**

```shell
#下载zookeeper镜像
docker pull wurstmeister/zookeeper
#启动zookeeper
docker run -d \
--name zookeeper \
--restart=always \
-p 2181:2181 \
-e TZ="Asia/Shanghai" \
wurstmeister/zookeeper:latest
#下载kafka镜像
docker pull wurstmeister/kafka
#启动kafka
docker run -d \
--name kafka \
--restart=always \
-p 9092:9092 \
-e KAFKA_BROKER_ID=0 \
-e KAFKA_ZOOKEEPER_CONNECT=192.168.188.100:2181 \
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://192.168.188.100:9092 \
-e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 \
-e TZ="Asia/Shanghai" \
wurstmeister/kafka:latest
#下载kafka-mannager镜像
docker pull sheepkiller/kafka-manager
#启动mannager
docker run -d \
--restart=always \
--name kfk-manager \
-p 9000:9000 \
-e ZK_HOSTS=192.168.188.100:2181 \
sheepkiller/kafka-manager:latest
#访问ip:9000
```



5、