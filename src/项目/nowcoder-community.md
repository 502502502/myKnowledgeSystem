#### 1、项目名称

nowcoder-community

#### 2、服务划分

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
ｖａａａａａａａａａａａｘｓｃｄｓｖｂｄｃｄｃｄｓｖｄｖｄｄｄｑｑｄｓｊｖｈｊ
```

