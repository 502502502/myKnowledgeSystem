[TOC]



# 一、Nosql

## 1、NoSQL特点

1.非关系型数据库，不依赖业务逻辑数据库存储，以简单key-value存储。因此大大的增加了数据库的扩展能力
2.不遵循SQL标准
3.不支持ACID

## 2、场景

**适合**

高并发读写
海量数据读写
数据可扩展



**不适合**

事务存储
复杂数据库

## 3、优点

+ 缓存数据库，完全在内存中，速度快，数据结构简单
+ 减少io操作，数据库和表拆分，虽然破坏业务逻辑，即外加一个缓存数据库，提高数据库速度，也可以用专门的存储方式，以及针对不同的数据结构存储

## 4、常见Nosql

**Memcache**	

数据都在内存中，一般不持久化

 key-value模式，支持类型单一 

一般是作为缓存数据库辅助持久化的数据库



**Redis**	

几乎覆盖了Memcached的绝大部分功能 

数据都在内存中，支持持久化，主要用作备份恢复

除了支持简单的key-value模式，还支持多种数据结构的存储，比如 list、set、hash、zset等

一般是作为缓存数据库辅助持久化的数据库



**MongoDB**	

高性能、开源、模式自由(schema free)的文档型数据库

数据都在内存中， 如果内存不足，把不常用的数据保存到硬盘

虽然是key-value模式，但是对value（尤其是json）提供了丰富的查询功能

支持二进制数据及大型对象/可以根据数据的特点替代RDBMS ，成为独立的数据库

配合RDBMS，存储特定的数据。





# 二、Redis

## 1、单线程+多路io口复用

在 Redis 只运行单线程的情况下，**该机制允许内核中，同 时存在多个监听套接字和已连接套接字**。内核会一直监听这些套接字上的连接请求或数据请求。一旦有请求到达，就会交给 Redis 线程处理，这就实现了一个 Redis 线程处理多个 IO 流的效果。

Redis 线程不会阻塞在某一个特定的监听或已连接套接字上，也就是说，不会阻塞在某一个特定的客户端请求处理 上。正因为此，Redis 可以同时和多个客户端连接并处理请求，从而提升并发性

## 2、环境配置

+ 下载传输Linux
+ 安装
+ 安装gcc
+ 启动
+ 修改配置文件



## 3、常用命令

**库的选择：**

- `select` 命令切换数据库
- `dbsize` 查看当前数据库的key数量
- `flushdb` 清空当前库
- `flushall` 通杀全部库



**key操作**

`keys * `查看当前库所有key

`set key value `设置key值与value

`exists key `判断key是否存在

`type key` 查看key是什么类型

`del key `删除指定的key数据

`unlink key`根据value选择非阻塞删除，仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作

`expire key` 10 10秒钟：为给定的key设置过期时间

`ttl key` 查看还有多少秒过期，-1表示永不过期，-2表示已过期

## 4、数据类型

### ①、string字符串

- 一个key对应一个value
- 二进制安全的，即可包含任何数据
- value最多可以是512m



**常用操作**

`set key value `设置key值
`get key `查询key值
`append key value` 将给定的value追加到原值末尾
`strlen key `获取值的长度
`setnx key value` 只有在key不存在的时候，设置key值
`incr key `将key值存储的数字增1，只对数字值操作，如果为空，新增值为1
`decr key` 将key值存储的数字减1，只对数字值操作，如果为空，新增值为1
`decr key `将key值存储的数字减1
`incrby/decrby key`<步长> 将key值存储的数字增减如步长



**补充操作**

`mset key value key value..`同时设置一个或者多个key-value
`mget key key...`同时获取一个或多个value
`msetnx key value key value..`同时设置一个或者多个key-value.当且仅当所有给定key都不存在
`getrange key `<起始位置> <结束位置> 获取key的起始位置和结束位置的值
`setrange key `<起始位置> value 将value的值覆盖起始位置开始
`setex key <> value` 设置键值的同时,设置过期时间
`getset key value `用新值换旧值



### ②、list列表

**常见操作**

`lpush/rpush key value value...`从左或者右插入一个或者多个值(头插与尾插)

`lpop/rpop key `从左或者右吐出一个或者多个值(值在键在,值都没,键都没)

`rpoplpush key1 key2 `从key1列表右边吐出一个值,插入到key2的左边

`lrange key start stop` 按照索引下标获取元素(从左到右)

`lrange key 0 -1 `获取所有值

`lindex key index `按照索引下标获得元素

`llen key `获取列表长度

`linsert key before/after value newvalue `在value的前面插入一个新值

`lrem key n value `从左边删除n个value值

`lset key index value `在列表key中的下标index中修改值value



### ③、set集合

+ 字典，哈希表
+ 自动排重且为无序的



**常见操作**

`sadd key value value... `将一个或者多个member元素加入集合key中,已经存在的member元素被忽略
`smembers key `取出该集合的所有值
`sismember key value `判断该集合key是否含有改值
`scard key` 返回该集合的元素个数
`srem key value value` 删除集合中的某个元素
`spop key` 随机从集合中取出一个元素
`srandmember key n` 随即从该集合中取出n个值，不会从集合中删除
`smove <一个集合a><一个集合b>value` 将一个集合a的某个value移动到另一个集合b
`sinter key1 key2` 返回两个集合的交集元素
`sunion key1 key2` 返回两个集合的并集元素
`sdiff key1 key2` 返回两个集合的差集元素（key1有的，key2没有）



### ④、hash哈希

+ 键值对集合，特别适合用于存储对象类型



**常见操作**

`hset key field value` 给key集合中的filed键赋值value
`hget key1 field` 集合field取出value
`hmset key1 field1 value1 field2 value2` 批量设置hash的值
`hexists key1 field` 查看哈希表key中，给定域field是否存在
`hkeys key` 列出该hash集合的所有field
`hvals key` 列出该hash集合的所有value
`hincrby key field increment` 为哈希表key中的域field的值加上增量1 -1
`hsetnx key field value` 将哈希表key中的域field的值设置为value，当且仅当域field不存在



### ⑤、Zset有序集合

+ 没有重复元素的字符串集合
+ 按照相关的分数进行排名
+ 排名从低到高
+ 排名可重复



**常见操作**

`zadd key score1 value1 score2 value2` 将一个或多个member元素及其score值加入到有序key中
`zrange key start stop (withscores)` 返回有序集key，下标在start与stop之间的元素，带withscores，可以让分数一起和值返回到结果集。
`zrangebyscore key min max(withscores)` 返回有序集key，所有score值介于min和max之间（包括等于min或max）的成员。有序集成员按score的值递增次序排列
`zrevrangebyscore key max min （withscores）`同上，改为从大到小排列
`zincrby key increment value` 为元素的score加上增量
`zrem key value` 删除该集合下，指定值的元素
`zcount key min max` 统计该集合，分数区间内的元素个数
`zrank key value` 返回该值在集合中的排名，从0开始



### ⑥、Bitmaps

+ 合理使用操作位可以有效地提高内存使用率和开发使用率
+ 本身是一个字符串，不是数据类型，数组的每个单元只能存放0和1，数组的下标在Bitmaps叫做偏移量
+ 节省空间，一般存储活跃用户比较多



**常见操作**

`setbit key offset value`	设置值

`getbit key offset` 	取值

`bitcount key （start end）`	统计数值

`bitop and(or/not/xor）destkey key`	交并非异或



### ⑦、HyperLogLog

+ 统计网页中页面访问量
+ 只会根据输入元素来计算基数，而不会储存输入元素本身，不能像集合那样，返回输入的各个元素
+ 基数估计是在误差可接受的范围内，快速计算（不重复元素的结算）



**常见操作**

`pfadd key element`	添加指定的元素到hyperloglog中

`pfcount key` 	计算key的近似基数

`pfmerge destkey sourcekey sourcekey`	一个或多个key合并后的结果存在另一个key



### ⑧、Geographic

+ 提供经纬度设置，查询范围，距离查询等



**常见操作**

geoadd key longitude latitude member	添加地理位置（经度纬度名称）

geopos key member 	获取两个位置之间的直线距离

georadius key longitude latitude radius (m km ft mi)	以给定的经纬度为中心，找出某一半径的内元素



## 5、访问控制ACL

ACL是Access Control List（访问控制列表）的缩写，该功能允许根据可以执行的命令和可以访问的键来限制某些连接。

 在Redis 5版本之前，Redis 安全规则只有密码控制 还有通过rename 来调整高危命令比如 flushdb ， KEYS* ， shutdown 等。Redis 6 则提供ACL的功能对用户进行更细粒度的权限控制：

+ 接入权限:用户名和密码

+ 可以执行的命令

+ 可以操作的 KEY



`acl list`命令展现用户权限列表
`acl cat`查看添加权限指令类别
`acl whoami`命令查看当前用户
`acl set user`命令创建和编辑用户ACL



## 6、IO多线程

IO多线程其实指客户端交互部分的网络IO交互处理模块多线程，而非执行命令多线程。Redis6执行命令依然是单线程

 Redis 6 加入多线程,但跟 Memcached 这种从 IO处理到数据访问多线程的实现模式有些差异。Redis 的多线程部分只是用来处理网络数据的读写和协议解析，执行命令仍然是单线程。



# 三、Jedis

## 1、配置

### ①、引入依赖

```xml
		<dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.6.1</version>
        </dependency>
```

```java
//创建Jedis对象
        Jedis jedis = new Jedis("192.168.242.110", 6379);
```



### ②、创建连接池

节省每次连接redis服务带来的消耗，把连接好的实例反复利用



**连接参数**

`MaxTotal`：控制一个pool可分配多少个jedis实例，通过pool.getResource()来获取；如果赋值为-1，则表示不限制；如果pool已经分配了MaxTotal个jedis实例，则此时pool的状态为exhausted。
`maxIdle`：控制一个pool最多有多少个状态为idle(空闲)的jedis实例；
`MaxWaitMillis`：表示当borrow一个jedis实例时，最大的等待毫秒数，如果超过等待时间，则直接抛JedisConnectionException；
`testOnBorrow`：获得一个jedis实例的时候是否检查连接可用性（ping()）；如果为true，则得到的jedis实例均是可用的；



**代码**

```java
public class JedisPoolUtil {
	private static volatile JedisPool jedisPool = null;

	private JedisPoolUtil() {
	}

	public static JedisPool getJedisPoolInstance() {
		if (null == jedisPool) {
			synchronized (JedisPoolUtil.class) {
				if (null == jedisPool) {
					JedisPoolConfig poolConfig = new JedisPoolConfig();
					poolConfig.setMaxTotal(200);
					poolConfig.setMaxIdle(32);
					poolConfig.setMaxWaitMillis(100*1000);
					poolConfig.setBlockWhenExhausted(true);
					poolConfig.setTestOnBorrow(true);  // ping  PONG，判断是否还存在
				 
					jedisPool = new JedisPool(poolConfig, "172.22.109.205", 6379, 60000 );
				}
			}
		}
		return jedisPool;
	}

	public static void release(JedisPool jedisPool, Jedis jedis) {
		if (null != jedis) {
			jedisPool.returnResource(jedis);
		}
	}

}

```



### ③、springboot整合

**pom.xml**

```xml
<!-- redis -->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<!-- spring2.X集成redis所需common-pool2-->
<dependency>
<groupId>org.apache.commons</groupId>
<artifactId>commons-pool2</artifactId>
<version>2.6.0</version>
</dependency>

```



**properties.yaml**

```yaml
#Redis服务器地址
spring.redis.host=172.22.109.205
#Redis服务器连接端口
spring.redis.port=6379
#Redis数据库索引（默认为0）
spring.redis.database= 0
#连接超时时间（毫秒）
spring.redis.timeout=1800000
#连接池最大连接数（使用负值表示没有限制）
spring.redis.lettuce.pool.max-active=20
#最大阻塞等待时间(负数表示没限制)
spring.redis.lettuce.pool.max-wait=-1
#连接池中的最大空闲连接
spring.redis.lettuce.pool.max-idle=5
#连接池中的最小空闲连接
spring.redis.lettuce.pool.min-idle=0
```



**RedisConfig**

```java
@EnableCaching
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setConnectionFactory(factory);
//key序列化方式
        template.setKeySerializer(redisSerializer);
//value序列化
        template.setValueSerializer(jackson2JsonRedisSerializer);
//value hashmap序列化
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
//解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
// 配置序列化（解决乱码的问题）,过期时间600秒
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(600))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();
        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}

```



**Test**

```java
		//设置值到redis
        redisTemplate.opsForValue().set("name","学习");
        //从redis获取值
        String name = (String)redisTemplate.opsForValue().get("name");
```





# 四、事务

## 1、悲观锁

悲观锁：不能同时进行多人，执行的时候先上锁。传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁

## 2、乐观锁

乐观锁：通过版本号一致与否，即给数据加上版本，同步更新数据以及加上版本号。不会上锁，判断版本号，可以多人操作，类似生活中的抢票。每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。乐观锁适用于多读的应用类型，这样可以提高吞吐量。Redis就是利用这种check-and-set机制实现事务的



| 命令    | 功能                               |
| ------- | ---------------------------------- |
| multi   | 组队阶段，还未执行                 |
| exec    | 执行阶段，将multi的队列放进 exec中 |
| discard | 放弃multi在队列中的值              |

## 3、ab测试

通过使用ab工具进行高并发测试

其具体参数设计，主要有几个比较重要

- -n 请求次数
- -c 当前请求次数的并发请求
- -T 设计的类型，可以是post，get
- -p 提交的参数



## 4、商品遗留问题

由于增加了乐观锁之后，假设一个人买了之后，版本改变了，下一个人都不能买了，所以出现了商品遗留的问题，都卖不出



解决该问题通过引入lua脚本

将复杂的或者多步的redis操作，写为一个脚本，一次提交给redis执行，减少反复连接redis的次数，提升性能。



LUA脚本是类似redis事务，有一定的原子性，不会被其他命令插队，可以完成一些redis事务性的操作，但是注意redis的lua脚本功能，只有在Redis 2.6以上的版本才可以使用。
利用lua脚本淘汰用户，解决超卖问题。
redis 2.6版本以后，通过lua脚本解决争抢问题，实际上是redis 利用其单线程的特性，用任务队列的方式解决多任务并发问题。



通过单线程任务排队的机制解决多个任务的高并发问题

![image-20220722110706322](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220722110706322.png)



# 五、持久化

## 1、RDB

### ①、概述

在指定的时间间隔内将内存中的数据集快照写入磁盘



**优点**：

+ 适合大规模的数据恢复
+ 对数据完整性和一致性要求不高更适合使用
+ 节省磁盘空间
+ 恢复速度快



**缺点**：

+ Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑
+ 虽然Redis在fork时使用了写时拷贝技术,但是如果数据庞大时还是比较消耗性能。
+ 在备份周期在一定间隔时间做一次备份，所以如果Redis意外down掉的话，就会丢失最后一次快照后的所有修改



### ②、备份流程

Redis会单独创建（fork）一个子进程来进行持久化，会先将数据写入到 一个临时文件中，待持久化过程都结束了，再用这个临时文件替换上次持久化好的文件。 整个过程中，主进程是不进行任何IO操作的，这就确保了极高的性能



### ③、特点

如果需要进行大规模数据的恢复，且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加的高效。

RDB的缺点是最后一次持久化后的数据可能丢失。
数据如果有变化的，会在/usr/local/bin目录下生成一个dum.rdb的文件



### ④、fork进程

Fork的作用是复制一个与当前进程一样的进程。

新进程的所有数据（变量、环境变量、程序计数器等） 数值都和原进程一致，但是是一个全新的进程，并作为原进程的子进程

在Linux程序中，fork()会产生一个和父进程完全相同的子进程，但子进程在此后多会exec系统调用，出于效率考虑，Linux中引入了“写时复制技术”

一般情况父进程和子进程会共用同一段物理内存，只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。

## 2、AOP

### ①、概述

以日志的形式来记录每个写操作（增量保存），将Redis执行过的所有写指令记录下来(读操作不记录)， 只许追加文件但不可以改写文件

redis启动之初会读取该文件重新构建数据，换言之，redis 重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作



优点：

- 备份机制更稳健，丢失数据概率更低
- 可读的日志文本，通过操作AOF稳健，可以处理误操作

缺点：

- 比起RDB占用更多的磁盘空间。
- 恢复备份速度要慢。
- 每次读写都同步的话，有一定的性能压力。
- 存在个别Bug，造成恢复不能



### ②、同步频率

`appendfsync always` 始终同步，每次Redis的写入都会立刻记入日志；性能较差但数据完整性比较好
`appendfsync everysec` 每秒同步，每秒记入日志一次，如果宕机，本秒的数据可能丢失。
`appendfsync no redis`不主动进行同步，把同步时机交给操作系统



### ③、Rewrite

AOF采用文件追加方式，文件会越来越大为避免出现此种情况，新增了重写机制, 当AOF文件的大小超过所设定的阈值时，Redis就会启动AOF文件的内容压缩， 只保留可以恢复数据的最小指令集



重写流程：

+ bgrewriteaof触发重写，判断是否当前有bgsave或bgrewriteaof在运行，如果有，则等待该命令结束后再继续执行。
+ 主进程fork出子进程执行重写操作，保证主进程不会阻塞。
+ 子进程遍历redis内存中数据到临时文件，客户端的写请求同时写入aof_buf缓冲区和aof_rewrite_buf重写缓冲区保证原AOF文件完整以及新AOF文件生成期间的新的数据修改动作不会丢失。
+ 文件重写完成
  + 子进程写完新的AOF文件后，向主进程发信号，父进程更新统计信息
  + 主进程把aof_rewrite_buf中的数据写入到新的AOF文件。
+ 使用新的AOF文件覆盖旧的AOF文件，完成AOF重写





# 六、主从复制

## 1、概述

主机数据更新后根据配置和策略， 自动同步到备机的master/slaver机制，Master以写为主，Slave以读为主



**意义**

- 读写分离，性能扩展
- 容灾快速恢复



## 2、运行机制

从机主动发送

+ Slave启动成功连接到master后，从机slave会发送一个sync命令
+ Master接到命令启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令， 在后台进程执行完毕之后，master将传送整个数据文件到slave,以完成一次完全同步
  + 全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。（刚开始从机连接主机，主机一次给）
  + 增量复制：Master继续将新的所有收集到的修改命令依次传给slave,完成同步 （主机修改了数据会给予从机修改的数据同步，叫做增量复制）
+ 断开之后重新连接，只要是重新连接master,一次完全同步（全量复制)将被自动执行，rdb的数据就会给从机。
  主机负责写，从机负责读



## 3、特殊情况



主机挂掉，执行shutdown
从机`info replication`还是显示其主机是挂掉的哪个

如果从机挂掉，执行shutdown
主机开始写数据，从机在开启的时候，恢复数据的时候是从主机从头开始追加的



上一个Slave可以是下一个slave的Master，Slave同样可以接收其他 slaves的连接和同步请求，那么该slave作为了链条中下一个的master, 可以有效减轻master的写压力,去中心化降低风险。



当一个master宕机后，后面的slave可以立刻升为master，其后面的slave不用做任何修改
可以使用命令：`slaveof no one` 将从机变为主机



## 5、哨兵模式

为了监控主机宕机之后，从机可以立马变为主机



**哨兵服务器**

```shell
sentinel monitor mymaster 127.0.0.1 6379 1
```

> 代码的含义为 `sentinel`哨兵，监控，一个id（别名），ip加端口号
> 其中mymaster为监控对象起的服务器名称， 1 为至少有多少个哨兵同意迁移的数量。



**运行机制**

![image-20220722112558449](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220722112558449.png)





# 七、集群

## 1、概述

Redis 集群实现了对Redis的水平扩容，即启动N个redis节点，将整个数据库分布存储在这N个节点中，每个节点存储总数据的1/N。
Redis 集群通过分区（partition）来提供一定程度的可用性（availability）： 即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求

无论从哪台主机写的数据，其他主机上都能读到数据



**优势：**

+ 实现扩容
+ 分摊压力
+ 无中心配置相对简单
  

**劣势：**

+ 多键操作是不被支持的
+ 多键的Redis事务是不被支持的。lua脚本不被支持
+ 由于集群方案出现较晚，很多公司已经采用了其他的集群方案，而代理或者客户端分片的方案想要迁移至redis cluster，需要整体迁移而不是逐步过渡，复杂度较大。
  

## 2、建立集群

**服务器**

```shell
cluster-enabled yes
cluster-config-file nodes-6391.conf
```



**创建集群**

```java
redis-cli --cluster create --cluster-replicas 1 192.168.242.110:6379 192.168.242.110:6380 192.168.242.110:6381 192.168.242.110:6389 192.168.242.110:6390 192.168.242.110:6391
```



**登录集群**

```java
redis-cli -c -p 6379
```



## 3、宕机

如果某个主机宕机了，从机上位变主机，之前那个主机上线之后，就会变成从机



那如果主从都宕机了，也就是负责该服务的主从都宕机了
就看具体的配置

`cluster-require-full-coverage`

为yes ，那么 ，整个集群都挂掉
为no ，那么，该插槽数据全都不能使用，也无法存储。



## 4、slots

一个 Redis 集群包含 16384 个插槽（hash slot）， 数据库中的每个键都属于这 16384 个插槽的其中一个

集群使用公式 CRC16(key) % 16384 来计算键 key 属于哪个槽

其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 

集群中的每个节点负责处理一部分插槽。



在redis-cli每次录入、查询键值，redis都会计算出该key应该送往的插槽，如果不是该客户端对应服务器的插槽，redis会报错，并告知应前往的redis实例地址和端口



- 查询集群中的值，`CLUSTER KEYSLOT k1`
- 查询卡槽中key的数量，`CLUSTER COUNTKEYSINSLOT 12706`
- 查询指定卡槽返回key的数量，`CLUSTER GETKEYSINSLOT 5474 2`





# 八、应用问题

## 1、缓存穿透

key对应的数据在数据源并不存在，每次针对此key的请求从缓存获取不到，请求都会压到数据源，从而可能压垮数据源。



**解决方案**

+ 设置空值缓存，而且设置超时时间
+ 通过bitmap的位运算进行存储，数据量比较小
+ 实时监控，将其禁止访问

## 2、缓存击穿

缓存过期，从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮



**解决方案**

+ 设置热门的key，加大时长过期

+ 实时监控调整

## 3、缓存雪崩

缓存集中过期，从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮



**解决方案**

+ 设置多个级别的缓存架构，时间来得及缓冲

+ 使用锁的机制

+ 设置一个过期时间标志来通知

+ 将过期时间分散，比如5分钟、5.01分钟等



## 4、分布式锁

### ①、概述

分布式系统多线程、多进程并且分布在不同机器上，这将使原单机部署情况下的并发控制锁策略失效

**解决方案：**

+ 基于数据库实现分布式锁

+ 基于缓存（Redis等），性能最高

+ 基于Zookeeper，可靠性最高



### ②、redis分布式锁

- 互斥性。在任意时刻，只有一个客户端能持有锁。

- 不会发生死锁。即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。

  > 设置过期时间

- 解铃还须系铃人。加锁和解锁必须是同一个客户端，客户端自己不能把别人加的锁给解了

  > 给锁设置UUID唯一标志

- 加锁和解锁必须具有原子性

  > 使用Lua脚本



