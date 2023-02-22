#### 1、项目名称

NowcoderCommunity

#### 2、项目搭建

```shell
#idea新建springboot项目

#上传新建github仓库

#新建develop分支

#从新分支克隆仓库到idea

#maven配置

#项目结构调整
```



#### 3、Docker配置环境

**安装centos**

```shell
#下载镜像

#安装centos7

#设置网络
1、vmware网络设置
2、虚拟机网络模式设置
3、虚拟机修改虚拟网卡配置
4、关闭防火墙
5、主机配置网络适配器
6、主机和虚拟机互ping

#连接xshell
```



**安装zip解压工具**

```shell
#安装zip解压工具
yum install -y unzip.x86_64
```



**wkhtmptopdf安装**

```shell
#下载依赖
yum install -y fontconfig libX11 libXext libXrender libjpeg libpng xorg-x11-fonts-75dpi xorg-x11-fonts-Type1
#下载
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox-0.12.6-1.centos7.x86_64.rpm
#解压
rpm -ivh wkhtmltox-0.12.6-1.centos7.x86_64.rpm
#安装gui工具
yum install -y xorg-x11-server-Xvfb.x86_64
#测试图片生成
xvfb-run --server-args="-screen 0, 1024x768x24" wkhtmltoimage https://www.baidu.com 1.png
#编写脚本
cd /opt
vi wkhtmltoimage.sh
xvfb-run --server-args="-screen 0, 1024x768x24" wkhtmltoimage "$@"
#执行权限
chmod +x wkhtmltoimage.sh
#测试脚本
cd /root
/opt/wkhtmltoimage.sh https://www.baidu.com 2.png
```



**安装JRE**

```shell
#安装jdk
yum install -y java-1.8.0-openjdk.x86_64
#查看
java -version
#安装java开发工具
yum install -y java-devel
```



**安装maven**

```shell
#官网下载tar.gz包，用xftp传到/mydata目录下后，解压
cd /mydata
ll
tar -zvxf apache-maven-3.8.6-bin.tar.gz -C /opt
#查看解压后的文件,复制文件名
cd /opt
ll
#修改环境变量
vi /etc/profile
#按下i,输入，粘贴复制的文件名
export PATH=$PATH:/opt/apache-maven-3.8.6/bin
#配置生效
source /etc/profile
#查看版本
mvn -version
#修改阿里云镜像仓库
cd /opt/apache-maven-3.8.6/conf
ll
vim settings.xml
#按i进入修改，注释原来的中央仓库，加上阿里云仓库
<mirrors>
    <mirror> 
    <id>alimaven</id> 
        <name>aliyun maven</name> 
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url> 
        <mirrorOf>central</mirrorOf> 
    </mirror> 
</mirrors>
#按下Esc, 输入：wq，回车
​```
```



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
docker cp mysql:/etc/mysql/ /mydata/mysql/conf
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
docker pull redis:7.0
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
-d redis:7.0 \
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
mkdir -p /mydata/es/data
mkdir -p /mydata/es/plugins
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
docker cp /mydata/es/plugins/ik elasticsearch:/usr/share/elasticsearch/plugins
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
-e KAFKA_ZOOKEEPER_CONNECT=192.168.176.100:2181 \
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://192.168.176.100:9092 \
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
-e ZK_HOSTS=192.168.176.100:2181 \
sheepkiller/kafka-manager:latest
#访问ip:9000
```



**安装tomcat**

```shell
#拉取镜像
 docker pull tomcat:8
#启动容器
docker run -d -p 8080:8080 --name tomcat tomcat:8
#复制配置文件
docker cp tomcat:/usr/local/tomcat/conf /mydata/tomcat/conf
docker cp tomcat:/usr/local/tomcat/logs /mydata/tomcat/logs
#启动容器
docker run -d -p 8080:8080 --name tomcat \
-v /mydata/tomcat/webapps:/usr/local/tomcat/webapps \
-v /mydata/tomcat/conf/conf:/usr/local/tomcat/conf \
-v /mydata/tomcat/logs/logs:/usr/local/tomcat/logs \
--restart=always tomcat:8
#访问主页
```



**安装nginx**

```shell
#下载镜像
docker pull nginx:latest
#启动容器
docker run --name nginx -p 80:80 -d nginx
#创建映射
mkdir -p /madata/nginx/conf.d 
mkdir -p /mydata/nginx/html
mkdir -p /mydata/nginx/logs
mkdir -p /mydata/nginx/conf
docker cp nginx:/etc/nginx/nginx.conf /mydata/nginx/conf
docker cp nginx:/etc/nginx/conf.d /mydata/nginx/conf.d
docker cp nginx:/usr/share/nginx/html /mydata/nginx
#重新启动
docker stop nginx 
docker rm nginx
docker run  -p 80:80 --name nginx --restart=always \
-v /mydata/nginx/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /mydata/nginx/conf.d:/etc/nginx/conf.d \
-v /mydata/nginx/html:/usr/share/nginx/html \
-v /mydata/nginx/logs:/var/log/nginx \
-d  nginx
#修改配置
vi /mydata/nginx/conf/nginx.conf
#注释原来的server,在http内新增如下，保存
    upstream myserver {
      	server 127.0.0.1:8080 max_fails=3 fail_timeout=30s;
    }
	server{
    	listen 80;
    	server_name 192.168.176.100;
    	location / {
            proxy_pass http://myserver;
     	}
	}
```



#### 4、数据库建立，连接

```shell
#导入sql文件后，解压
unzip -d /mydata/ dbSql.zip
docker cp /mydata/dbSql mysql:/tmp
#进入mysql
docker exec -it mysql /bin/bash
#登录账号，输入密码
mysql -u root -p
mysql> source /tmp/dbSql/db.sql;
mysql> source /tmp/dbSql/data.sql;
mysql> source /tmp/dbSql/quartz.sql;
#查看数据库和表
mysql> show databases;
mysql> use community;
mysql> show tables;
mysql> select * from user;

#idea连接数据库
```



#### 5、mybatics配置



#### 6、thyleaf配置



#### 7、静态资源整理



#### 8、首页搭建



9、



10、