## 一、项目过程



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



**安装jdk**

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
docker run -p 3306:3306 --name mysql --restart=always \
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
```



```shell
#拉取镜像
docker pull mysql:5.6.51
#运行并挂载
docker run -p 3308:3308 --name mysql5 --restart=always \
-v /mydata/mysql5/log:/var/log/mysql \
-v /mydata/mysql5/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
-d mysql:5.6.51

#查看状态
docker ps
docker ps -a

#添加配置文件
docker cp mysql5:/etc/mysql/ /mydata/mysql5/conf
cd /mydata/mysql5/conf
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
-e KAFKA_ZOOKEEPER_CONNECT=192.168.17.100:2181 \
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://192.168.17.100:9092 \
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
-e ZK_HOSTS=192.168.17.100:2181 \
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



## 二、项目总结

#### 1、介绍一下项目

项目的整体结构来源于牛客网，基于springboot构建，主要实现了一个社区互动的功能，包括用户的注册登录，权限控制，用户发帖，浏览帖子，对帖子进行评论，回复，点赞，对其他用户进行关注和私信，以及系统通知，帖子搜索，热榜排行，管理员对帖子进行加管理和统计数据等功能。

在技术栈上，前端使用thyleaf作为模板引擎，使用ajax实现一些异步请求的发送，后端使用springboot搭建项目，使用maven进行依赖管理，使用github进行版本控制，使用mysql存储网站数据，使用mybatics-plus实现mysql的存取等操作，使用redis还有本地缓存存储频繁存取的数据，使用kafka来处理一些后台的事件，使用elasticsearch做了一个全文搜索，使用springsecurity对不同的用户以及状态进行权限管理，使用对象存储作为网站的图床，使用quartz实现定时任务，去更新帖子的热榜，去更新全文搜索的内容，使用docker来将项目部署到linux环境。



#### 2、权限管理是如何实现的

用户有多种身份，在数据库中通过一个字段来标识，使用自己的登录逻辑，构建一个用户认证的结果。

使用security根据用户认证实现不同权限的用户可以访问不同的服务，并处理权限不足的访问，以及在页面上控制不同的展示。	



#### 3、搜索功能如何实现

将帖子存储到elasticsearch里边，配置elasticsearch的分词器，对查询的内容进行分词，然后进行内容检索，将检索的结果进行处理，对分词后的关键字进行高亮显示，最后将信息整合，做一下排序，响应给前端。

当帖子进行了更新和删除的时候，会更新es里的内容。



#### 4、如何统一异常处理

使用ControllerAdvice进行统一异常处理，拦截所有的controller响应，当controller发生异常时，捕获，根据请求的类型响应错误信息，并且将错误日志进行记录。



#### 5、如何统一日志

使用Aspect对service做一个AOP增强，配置需要记录日志的切入点，编写一个在方法之前执行的切面进行日志的记录，当调用切入点时，会先执行切面，记录访问日志。



#### 6、事务是如何实现的

在service层使用注解@Transactional，设置隔离级别和传播级别



#### 7、敏感词过滤是如何实现的

将创建一棵敏感词的前缀树，对字符串进行过滤，以每一个字符为起点，在前缀树中检索关键词，若是关键词则使用**号代替，最后输出过滤后的字符串。

前缀树利用字符串的公共前缀来减少查询时间，最大限度地减少无谓的字符串比较。



#### 8、注册是怎么做的

前端通过表单提交数据，对表单数据进行格式检查，然后进行用户的查重，如果是新用户就将用户信息存到mysql，在数据库中使用一个字段存储用户的激活状态，注册后需要验证邮箱，使用JavaMailSender向用户发送邮件，用户点击激活后将数据库里激活状态字段值进行更新即可。



#### 9、评论、点赞、关注是如何实现的

评论存储在mysql，点赞和关注信息都是存储到redis，方便存取，查询点赞和关注信息时也都是从redis获取数据。

评论有两种，一种是评论帖子的，一种是回复给评论的，每个评论都保存有它评论的对象，在插入记录时使用事务同时更新被评论对象的相关信息。

评论点赞和专注都会触发事件机制，生成事件后到消息队列kafka排队，当消费到它时，会将事件的相关信息提取出来，给事件关联的对象发送一个系统通知，告诉它有人对他的帖子进行了评论，点赞还要关注。



#### 10、私信、通知是如何实现的

私信和通知本质上是一样的，只不过通知的发送者是系统，消息存储在mysql数据库，当用户浏览时会在数据库表中进行查询。

在浏览私信和通知的时候，会在私信和通知列表显示未读消息的数量，当信息被浏览时，会将看到的信息设置为已读，更新到数据库。

私信是以会话为单位进行组织和管理，两个用户之间是一个对话。会话列表和消息列表都使用了分页管理，系统通知进行了分类。

系统通知的发送是通过消息队列Kafka来完成的，可以显著提高网站的响应速度，在后台空闲时消费，发送通知；

发送私信则是一个同步的请求，在对数据库进行更新后才会响应



#### 11、定时任务是如何实现的

因为帖子的热度在实时变化，因此需要定时去更新帖子热度，更新数据库，还有es;

在引起热度变化的事件发生时，将需要更新热度的帖子的id存入redis，然后按照一定的时间去查看是否有需要更新的帖子，如果有就执行更新，将最新的帖子更新到mysql，还有es。

使用Quartz模块实现定时任务，配置一下mysql，设置定时执行的时间，使用的线程数量等，然后编写执行任务的逻辑，根据公式计算帖子的热度，然后更新mysql以及es。



quartz是一个开源项目，完全基于java实现。是一个优秀的开源调度框架。
特点：
1，强大的调度功能，例如支持丰富多样的调度方法
2，灵活的应用方式，例如支持任务和调度的多种组合方式
3，分布式和集群能力
专业术语：
scheduler：任务调度器 ， scheduler是一个计划调度器容器，容器里面有众多的JobDetail和trigger，当容器启动后，里面的每个JobDetail都会根据trigger按部就班自动去执行
trigger：触发器，用于定义任务调度时间规则
job：任务，即被调度的任务， 主要有两种类型的 job：无状态的（stateless）和有状态的（stateful）。一个 job 可以被多个 trigger 关联，但是一个 trigger 只能关联一个 job
misfire：本来应该被执行但实际没有被执行的任务调度





#### 12、如何提升网站性能

首先是使用redis存储一些频繁操作的数据，redis的存取效率远高于mysq，其次利用redis代替session存储一些临时数据；



对于一些变化不大，但是经常使用的数据，使用本地缓存Caffeine，保存在服务器的机器上，降低响应的时间，比如将高热度的帖子保存在本地缓存，在查找的时候直接获取，不需要访问数据库和redis，极大提高效率。

Caffeine 是一个基于Java 8的高性能本地缓存框架
初始化cache：缓存保存的对象，使用Caffeine.newBuilder()创建，创建时设置缓存大小，过期时间，缓存未命中时的加载方式。



使用消息队列，将一些后台工作暂时保存起来，服务器空闲时处理，提高系统的响应速度



#### 13、数据统计如何实现

使用redis快速存储，统计，使用拦截器对请求进行分析记录即可

可以对请求的IP进行统计，也可以对请求所属的用户进行统计。

利用redis的HyperLogLog数据结构实现快速计数。



#### 14、部署是怎么做的

在linux环境部署项目，使用docker快速安装mysql，redis，es，kafka等服务，由于机器性能有限，以及项目规模不大，选择了部署到同一个虚拟机，使用maven打包，tomcat作为服务器，使用nginx做代理。



#### 15、难点在哪些地方

在部署方面，在刚开始，使用云服务器部署，没有使用docker，就是一个服务一个服务的装上去，改配置，有时候出错了，就查看日志，鼓捣了好几天才跑通；后来使用docker部署后就好多了。

在模块的选择方面，由于各个模块的对应版本要求都不太一样，在选择elasticsearch的时候就冲突过，调试的过程还是比较费劲的。

前端页面的编写还是比较困难的，因为没有那么多提示，而且对于前端的代码经验不是很多，常常鼓捣半天搞不出想要的效果，也找不到哪里出的毛病，比如有一次通过ajax发送异步请求，响应函数不能成功执行，测试后端又是成功返回的，后来才想明白是异步的原因，执行响应函数的时候还没有返回结果，改成同步的就解决了问题；

在处理前后端参数传递的时候，对于几种传参方式，请求路径带参数，restful风格，异步请求传参，form表单穿数据，接受参数的方式有些小的差异，什么时候可以用对象接受，什么时候需要加注解，当时是比较迷惑的。







#### 16、哪些方面可以优化

服务器主动推送(消息，私信)，前端可以获取实时数据，特别是私信模块；

可以将服务部署到多个服务器上，提高稳定性和系统带宽。

管理员的权限还不支持申请，可以开通这个成为管理员，或者版主的渠道

在帖子的详情页面可以设置一个返回的功能，返回进入详情之前的页面，可能是分页展示的其中一页，也可能是全文搜索的结果；

全文搜索应该提供一个过滤的条件选择，类似引流，发帖也可以为自己的帖子设置引流；

发帖和私信可以优化支持图片，表情包，文件等多元化的信息；

帖子可以支持后期修改，以及删除自己的帖子；

实现帖子浏览量的一个统计，以及在评论区显示楼主以及作者的一个标签；

可以提供一个举报帖子和用户的选项，管理员多设置一个页面来管理这些举报内容；

帖子评论的回复过多的时候可以选择折叠；



#### 17、登录是怎么实现的

对于用户的首次登录需要进行账号密码的检查，以及验证码的核检，验证码是使用kaptcha生成的，然后存储到redis，并将key值存到cooike中，当用户登录的时候从cookie取出key，再从redis取出验证码进行比对。

成功登录后会生成一个登录凭证，保存到redis中，登录凭证没过期之前，都是不需要重新进行登录的，编写一个拦截器对所有需要登录操作的请求进行拦截，从redis取出ticket检查是否过期。

