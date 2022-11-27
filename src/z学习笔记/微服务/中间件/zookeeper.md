# 一、概述

## 1、简介

为分布式应用提供协调服务



 从设计模式角度来理解：是一个基于观案者模式设计的分布式服务管理框架，它负责存储和管理大家都关心的数据，然后接受观察者的注册，一旦这些数据的状态发生变化，Zookeeper 就将负责通知已经在 Zookeeper 上注册的那些观察者做出相应的反应。



## 2、数据结构

ZooKeeper 数据模型的结构与 Unix 文件系统很类似，整体上可以看作是一棵树，每个节点称做一个 ZNode。每一个 ZNode 默认能够存储 1 MB 的数据，每个 ZNode 都可以通过其路径唯一标识。



## 3、应用场景

**统一命名服务**

在分布式环境下，经常需要对应用/服务进行统一命名 ，便于识别。例如：IP 不容易记住，而域名容易记住。



**统一配置管理**

+ 分布式环境下，配置文件同步非常常见。
  + 一般要求一个集群中，所有节点的配置信息是一致的，比如 Kafka 集群。
  + 对配置文件修改后，希望能够快速同步到各个节点上。

+ 配置管理可交由 ZooKeeper 实现。
  + 可将配置信息写入 ZooKeeper 上的一个 Znode 。
  +  各个客户端服务器监听这个 Znode。
  +  一旦 Znode 中的数据被修改，ZooKeeper 将通知各个客户端服务器。





**统一集群管理**

+ 分布式环境中，实时掌握每个节点的状态是必要的。
  + 可根据节点实时状态做出一些调整。

+ ZooKeeper 可以实现实时监控节点状态变化
  + 可将节点信息写入ZooKeeper 上的一个 ZNode。
  +  监听这个 ZNode 可获取它的实时状态变化。



**服务器动态上下线**



**软负载均衡**

在 Zookeeper 中记录每台服务器的访问数，让访问数最少的服务器去处理最新的客户端请求。





# 二、安装使用

## 1、安装

**准备工作**

+ 拷贝 Zookeeper 安装包到 Linux 系统下（apache-zookeeper-3.5.6-bin.tar.gz）
+  解压到指定目录

+ 重命名



**修改配置**


**修改环境变量**


**操作 Zookeeper**

+ 启动服务
+ 关闭服务
+ 查看状态
+ 启动客户端
+ 关闭客户端



## 2、创建集群

**配置服务器编号**



**配置 zoo.cfg 文件**

`tickTime =2000：`通信心跳数，Zookeeper 服务器与客户端心跳时间，单位毫秒

Zookeeper 使用的基本时间，服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个tickTime 时间就会发送一个心跳，时间单位为毫秒。它用于心跳机制，并且设置最小的 session 超时时间为两倍心跳时间。(session 的最小超时时间是 2*tickTime）

`initLimit =10：`LF 初始通信时限

集群中的 Follower 跟随者服务器与 Leader 领导者服务器之间初始连接时能容忍的最多心跳数（tickTime的数量），用它来限定集群中的 Zookeeper 服务器连接到 Leader 的时限。

`syncLimit =5：`LF 同步通信时限

集群中 Leader 与 Follower 之间的最大响应时间单位，假如响应超过 syncLimit * tickTime，Leader 认为 Follwer 死掉，从服务器列表中删除 Follwer。

`dataDir：`数据文件目录+数据持久化路径

主要用于保存 Zookeeper 中的数据。

`clientPort =2181：`客户端连接端口

监听客户端连接的端口。

`server.A=B:C:D`

A 是一个数字，表示这个是第几号服务器；集群模式下配置一个文件 myid，这个文件在 dataDir 目录下，这个文件里面有一个数据就是 A 的值，Zookeeper 启动时读取此文件，拿到里面的数据与 zoo.cfg 里面的配置信息比较从而判断到底是哪个server。
B 是这个服务器的 ip 地址；
C 是这个服务器与集群中的 Leader 服务器交换信息的端口；
D 是万一集群中的 Leader 服务器挂了，需要一个端口来重新进行选举，选出一个新的 Leader，而这个端口就是用来执行选举时服务器相互通信的端口。



**修改环境变量**



**启动 Zookeeper**



## 3、常用命令


`zkCli.sh`	启动客户端

`help`	显示所有操作命令

`ls /`	查看当前 znode 中所包含的内容

`ls -s /`	查看当前节点详细数据

`create /animals "dog"`	创建普通节点

`get /animals`	获得节点的值

 `create -e /animals/big "elephant"`	创建短暂节点

`create -s /animals/middle "hourse"`	创建带序号的节点

`set /animals/small "bug"`	修改节点数据值

`delete /animals/big`	删除节点

`deleteall /animals/mini`	递归删除节点

`stat /animals`	查看节点状态



# 三、内部原理

## 1、选举机制

**服务器ID**

比如有三台服务器，编号分别是1,2,3。

> 编号越大在选择算法中的权重越大。

**数据ID**

服务器中存放的最大数据ID.

> 值越大说明数据越新，在选举算法中数据越新权重越大。

**逻辑时钟**

或者叫投票的次数，同一轮投票过程中的逻辑时钟值是相同的。每投完一次票这个数据就会增加，然后与接收到的其它服务器返回的投票信息中的数值相比，根据不同的值做出不同的判断。



**选举状态**

- LOOKING，竞选状态。
- FOLLOWING，随从状态，同步leader状态，参与投票。
- OBSERVING，观察状态,同步leader状态，不参与投票。
- LEADING，领导者状态。

**选举消息内容**

在投票完成后，需要将投票信息发送给集群中的所有服务器，它包含如下内容。

- 服务器ID
- 数据ID
- 逻辑时钟
- 选举状态



**判断是否已经胜出**

默认是采用投票数大于半数则胜出的逻辑。



**选举流程简述**

目前有5台服务器，每台服务器均没有数据，它们的编号分别是1,2,3,4,5,按编号依次启动，它们的选择举过程如下：

- 服务器1启动，给自己投票，然后发投票信息，由于其它机器还没有启动所以它收不到反馈信息，服务器1的状态一直属于Looking。
- 服务器2启动，给自己投票，同时与之前启动的服务器1交换结果，由于服务器2的编号大所以服务器2胜出，但此时投票数没有大于半数，所以两个服务器的状态依然是LOOKING。
- 服务器3启动，给自己投票，同时与之前启动的服务器1,2交换信息，由于服务器3的编号最大所以服务器3胜出，此时投票数正好大于半数，所以服务器3成为领导者，服务器1,2成为小弟。
- 服务器4启动，给自己投票，同时与之前启动的服务器1,2,3交换信息，尽管服务器4的编号大，但之前服务器3已经胜出，所以服务器4只能成为小弟。
- 服务器5启动，后面的逻辑同服务器4成为小弟。





## 2、数据同步

**zookeeper服务器集群存在三种节点型**

`Leader（领导者）：`各个节点之间的老大，是集群中的核心。没有leader集群将不能工作。所有的写请求最终都会转交给领导者Leader执行；与跟随者（Follower）和观察者(Observer)进行心跳连接;数据同步到Follower和Observer。

`Follower(追随者)：`跟随者自己不会执行写的操作，而是会将写的操作转交给Leader执行。负责处理Leader发来的请求和数据，当Leader宕机之后，会进行投票和选举（从剩余的Follwer）,从而选举新的Leader。

`observer（观察者）：`Observer同Follwer一样，也不能执行写操作，他的功能跟Follwer差不多。observer为用于提高读取吞吐量，减少选举的时候而生，因此Observer不能参与投票和选举。



**数据同步流程**

leader 接受到消息请求后，将消息赋予给一个全局唯一的64位自增id，叫：zxid。
leader 为每个follower 准备了一个FIFO队列（通过TCP协议来实现，以实现了全局有序这个特点）将带有zxid的消息作为一个提案（proposal）分发给所有的follower。
当follower接受到proposal，先把proposal写到磁盘，写入成功以后再向leader恢复一个ack
当leader 接受到合法数量（超过半数节点）的 ack，leader 就会向这些follower发送commit命令，同时会在本地执行该消息
当follower接受到消息的commit命令以后，就会提交该消息。



## 3、CAP

**CAP概述**

一个分布式系统不可能同时满足以下三种

- 一致性（C:Consistency）
- 可用性（A:Available）
- 分区容错性（P:Partition Tolerance）

这三个基本需求，最多只能同时满足其中的两项，**因为P是必须的,因此往往选择就在CP或者AP中**。



**一致性（C:Consistency）**
例如一个将数据副本分布在不同分布式节点上的系统来说，如果对第一个节点的数据进行了更新操作并且更新成功后，其他节点上的数据也应该得到更新，并且所有用户都可以读取到其最新的值，那么这样的系统就被认为具有强一致性（或严格的一致性，最终一致性）



**可用性（A:Available）**
可用性是指系统提供的服务必须一直处于可用的状态，对于用户的每一个操作请求总是能够在有限的时间内返回结果。“有效的时间内”是指，对于用户的一个操作请求，系统必须能够在指定的时间（即响应时间）内返回对应的处理结果，如果超过了这个时间范围，那么系统就被认为是不可用的。



**分区容错性（P:Partition Tolerance）**
分区容错性约束了一个分布式系统需要具有如下特性：分布式系统在遇到任何网络分区故障的时候，仍然需要能够保证对外提供满足一致性和可用性的服务，除非是整个网络环境都发生了故障。



**一致性分类**

强一致性：（strong consistency）：任何时刻，任何用户都能读取到最近一次成功更新的数据。

单调一致性：（monotonic consistency）：任何时刻，任何用户一旦读到某个数据在某次更新后的值，那么就不会再读到比这个值更旧的值。也就是说，可获取的数据顺序必是单调递增的。

会话一致性：（session consistency）：任何用户在某次会话中，一旦读到某个数据在某次更新后的值，那么在本次会话中就不会再读到比这值更旧的值，会话一致性是在单调一致性的基础上进一步放松约束，只保证单个用户单个会话内的单调性，在不同用户或同一用户不同会话间则没有保障。

最终一致性：（eventual consistency）：用户只能读到某次更新后的值，但系统保证数据将最终达到完全一致的状态，只是所需时间不能保障。

弱一致性：（weak consistency）：用户无法在确定时间内读到最新更新的值。



**ZooKeeper保证的是CP**

不能保证每次服务请求的可用性

进行leader选举时集群都是不可用

顺序一致性：来自任意特定客户端的更新都会按其发送顺序被提交保持一致



## 4、节点类型

`PERSISTENT：`持久化，不会随客户端的断开而自动删除,默认类型
`PERSISTENT_SEQUENTIAL：`带序号的持久化，znode的名字将被附加一个单调递增的数字
`EPHEMERAL：`临时节点，当客户端断开时自动删除
`EPHEMERAL_SEQUENTIAL：`带序号的临时节点，znode的名字将被附加一个单调递增的数字
`CONTAINER：`container节点是一个特殊用途的节点，对于诸如leader、lock等非常有用。当容器的最后一个子对象被删除时，该容器将成为将来某个时候由服务器删除的候选对象。
`PERSISTENT_WITH_TTL：`zookeeper的扩展类型，如果znode在给定的TTL内没有被修改，它将在没有子节点时被删除。要想使用该类型，必须在zookeeper的bin/zkService.sh中的启动zookeeper的java环境中设置环境变量zookeeper.extendedTypesEnabled=true（具体做法在下边），否则KeeperErrorCode = Unimplemented for /**。
`PERSISTENT_SEQUENTIAL_WITH_TTL：`同上，是不过是带序号的



## 5、start结构体

`czxid：` 创建节点的事务 zxid

每次修改 ZooKeeper 状态都会收到一个 zxid 形式的时间戳，也就是 ZooKeepe r事务 ID。
事务 ID 是 ZooKeeper 中所有修改总的次序。每个修改都有唯一的 zxid，若 zxid1 小于 zxid2，那么 zxid1 在 zxid2 之前发生。

`ctime：` znode 被创建的毫秒数（从 1970 年开始）

`mzxid：` znode 最后更新的事务 zxid

`mtime：` znode 最后修改的毫秒数（从 1970 年开始）

`pZxid：` znode 最后更新的子节点 zxid

`cversion ：` znode 子节点变化号，znode 子节点修改次数

`dataversion：` znode 数据变化号

`aclVersion：` znode 访问控制列表的变化号

`ephemeralOwner：` 如果是临时节点，这个是 znode 拥有者的 session id。如果不是临时节点则是 0。

`dataLength：` znode 的数据长度

`numChildren：` znode 子节点数量



## 6、分布式锁

使用临时顺序节点是可以实现分布式锁



首先假如第一个客户端来获取共享资源也就是获取锁时，zookeeper客户端会创建持久根节点/locks

![img](https://ningct.oss-cn-hangzhou.aliyuncs.com/v2-3c8363582103b07a398d0d6c0bdc57f2_720w.jpg)



这个时候，客户端Client1就会查询/locks节点下面所有子节点，然后判断自己的节点是不是排序最小的那个，此时，如果是最小的则会获得锁，就能够对共享资源进行操作。

![img](https://ningct.oss-cn-hangzhou.aliyuncs.com/v2-6a0c091ced80c79667a212664d16198e_720w.jpg)

如果，这个时候又来个个客户端Client2也来尝试获取锁，那么它也会在zookeeper的/locks节点下创建一个节点。

![img](https://ningct.oss-cn-hangzhou.aliyuncs.com/v2-fc928a893a9a0e31185a19d9d74dbac4_720w.jpg)



Client2同样也会查询zookeeper中/locks节点下所有节点，判断自己编号是不是最小的，此时，发现自己并不是最小的，所以获取锁失败，然后就像它的前面一位节点0001注册Watcher事件来监控0001节点是否存在。

![img](https://ningct.oss-cn-hangzhou.aliyuncs.com/v2-27c4c8ffc609263d18d9cea79d558958_720w.jpg)

## 7、监听机制

**介绍**

在Zookeeper中所有的读操作都可以设置监听，它可以实时监听节点和子节点的变化，一旦发生变化将通知到客户端。
这个Watch是一次性的触发，当设置了Watch的数据发生改变时，则服务器将会把这个改变发送给设置了该watch的客户端。



**监听内容**

可以设置两种监听：数据监听和子节点监听。getData()和exists()中设置的监听是数据监听，getChildren()中设置的监听是子节点监听。

setData()将为正在设置的znode触发数据监视(假设设置成功)。一个成功的create()将为创建的znode触发一个数据监视，并为父znode触发一个子监视。

一个成功的delete()将为一个被删除的znode触发一个数据监听和一个子监听(因为没有更多的子监听了)，以及一个父znode的子监听。



**持久递归的监听**
从3.6开始可以设置一个持久监听和一个持久的并递归到子节点的监听。在监听被触发时不会被删除。这三种类型会被触发：NodeCreated, NodeDeleted, and NodeDataChanged 。 意思是当节点创建、节点删除、节点数据更新时会触发，同时也包含了子节点的创建、删除、和数据更新。这个事件是递归到每个子节点上的。即使节点被删除了，这个监听还是存在的。



**移除监听**
可以使用removewatches /path移除监听。

监听移除后触发的事件：

+ 子节点监听移除事件 ChildWatchRemoved

+ 数据监听移除事件 DataWatchRemoved

+ 持久监听移除事件 PersistentWatchRemoved
  



# 四、Java应用

## 1、环境搭建

**引入依赖**

Curator的jar包已经发布到Maven中心，由以下几个artifact的组成。根据需要选择引入具体的artifact。但大多数情况下只用引入curator-recipes即可。

curator-recipes	所有典型应用场景。需要依赖client和framework，需设置自动获取依赖。
curator-framework	同组件中framework介绍。
curator-client	同组件中client介绍。
curator-test	包含TestingServer、TestingCluster和一些测试工具。
curator-examples	各种使用Curator特性的案例。
curator-x-discovery	在framework上构建的服务发现实现。
curator-x-discoveryserver	可以和Curator Discovery一起使用的RESTful服务器。
curator-x-rpc	Curator framework和recipes非java环境的桥接。



**项目组件**

Recipes	Zookeeper典型应用场景的实现，这些实现是基于Curator Framework。
Framework	Zookeeper API的高层封装，大大简化Zookeeper客户端编程，添加了例如Zookeeper连接管理、重试机制等。
Utilities	为Zookeeper提供的各种实用程序。
Client	Zookeeper client的封装，用于取代原生的Zookeeper客户端（ZooKeeper类），提供一些非常有用的客户端特性。
Errors	Curator如何处理错误，连接问题，可恢复的例外等。



**配置类**

****

## 2、API

**创建客户端**

```java
RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000,3);
CuratorFramework client = CuratorFrameworkFactory.newClient("127.0.0.1:2181",retryPolicy);
client.start();
```



**创建节点**

```java
//创建一个初始内容为空的节点
client.create().forPath(path);
//创建一个包含内容的节点
client.create().forPath(path,"我是内容".getBytes());
//创建临时节点，并递归创建父节点
client.create().creatingParentsIfNeeded().withMode(CreateMode.EPHEMERAL).forPath(path);
//此处Curator和ZkClient一样封装了递归创建父节点的方法。在递归创建父节点时，父节点为持久节点。
```



**删除节点**

```java
//删除一个子节点
client.delete().forPath(path);
//删除节点并递归删除其子节点
client.delete().deletingChildrenIfNeeded().forPath(path);
//指定版本进行删除
client.delete().withVersion(1).forPath(path);
//如果版本不存在，则删除异常，信息如下：
org.apache.zookeeper.KeeperException$BadVersionException: KeeperErrorCode = BadVersion for
//强制保证删除一个节点
client.delete().guaranteed().forPath(path);
```



**读取数据**
读取节点数据内容API相当简单，Curator提供了传入一个Stat，使用节点当前的Stat替换到传入的Stat的方法，查询方法执行完成之后，Stat引用已经执行当前最新的节点Stat。

```java
// 普通查询
client.getData().forPath(path);

// 包含状态查询
Stat stat = new Stat();
client.getData().storingStatIn(stat()).forPath(path);
```



**更新数据**
更新数据，如果未传入version参数，那么更新当前最新版本，如果传入version则更新指定version，如果version已经变更，则抛出异常。

```java
// 普通更新
client.setData().forPath(path,"新内容".getBytes());

// 指定版本更新
client.setData().withVersion(1).forPath(path);
```



**监听器**

Curator提供了三种Watcher(Cache)来监听结点的变化：

+ Path Cache：监视一个路径下1）孩子结点的创建、2）删除，3）以及结点数据的更新。产生的事件会传递给注册的PathChildrenCacheListener。

+ Node Cache：监视一个结点的创建、更新、删除，并将结点的数据缓存在本地。

+ Tree Cache：Path Cache和Node Cache的“合体”，监视路径下的创建、更新、删除事件，并缓存路径下所有孩子结点的数据。
  

**分布式锁**