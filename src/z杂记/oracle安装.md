```shell
#安装镜像
docker pull registry.aliyuncs.com/helowin/oracle_11g

#启动容器
docker run  -itd -p 1521:1521 --name oracle_11g --restart=always \
-v /mydata/oracle/app:/opt/oracle/app \
-v /mydata/oracle/dpdump:/opt/oracle/dpdump  \
-v  /mydata/oracle/oraInventory:/opt/oracle/oraInventory registry.aliyuncs.com/helowin/oracle_11g

#进入oracle
docker exec -it oracle_11g bash
#登录root
su root
#修改配置文件
vi /etc/profile
61 export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
63 export ORACLE_SID=helowin
65 export PATH=$ORACLE_HOME/bin:$PATH
#配置生效
source  /etc/profile
#创建软连接
ln -s $ORACLE_HOME/bin/sqlplus /usr/bin

#切换用户
su - oracle
#登录sqlplus
sqlplus /nolog
#修改sys、system用户密码
SQL> alter user system identified by 123456;
SQL> alter user sys identified by 123456;
SQL> alter profile default limit PASSWORD_LIFE_TIME UNLIMITED;
#创建用户
SQL> create user root identified by 123456;
SQL> grant connect,resource,dba to root;
# 查看当前用户
SQL> show user;      
USER is "SYS"
#退出
SQL> exit

#查看实例状态
lsnrctl status
```

