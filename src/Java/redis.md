### 1、Redis是什么？简述它的优缺点？

﻿Redis本质上是一个Key-Value类型的内存数据库，整个数据库加载在内存当中操作，定期通过异步操作把数据库中的数据flush到硬盘上进行保存。

Redis的优点有：

- Redis速度快，适合缓存数据库查询、复杂计算、API调用和会话状态
- Redis支持数据持久化，可以将内存中的数据保存到磁盘上，防止数据丢失
- Redis支持发布订阅模式，可以实现消息的实时传递和处理
- Redis支持事务和流水线操作，可以保证数据的一致性和效率
- Redis支持多种数据类型

Redis的缺点有：
- Redis占用内存较大，需要根据数据量和业务需求合理分配内存资源
- Redis是单线程模型，CPU密集型操作会影响其性能
- Redis不支持复杂查询和关联操作，如果需要这些功能，可能需要使用其他数据库或中间件



### 2、redis为什么这么快

- Redis是基于内存的，避免了磁盘I/O等耗时操作
- Redis命令处理的核心模块为单线程，减少了锁竞争，以及频繁创建线程和销毁线程的代价，减少了线程上下文切换的消耗
- Redis使用多路复用技术，可以处理并发的连接²。内部实现采用epoll，采用了epoll+自己实现的简单的事件框架
- Redis数据结构简单，对数据操作也简单。Redis中的数据结构是专门进行设计的



### 3、Redis相比Memcached有哪些优势

- Redis支持更丰富的数据类型，不仅仅是字符串，还有列表，集合，有序集合，哈希等
- Redis可以持久化数据，支持RDB和AOF两种方式
- Redis支持主从复制和哨兵机制，提高了数据的可靠性和可用性
- Redis可以处理一些简单的逻辑运算，比如对集合进行交并差等操作



### 4、为什么用redis做缓存

- Redis基于内存，提供了高性能的数据存取功能，减少磁盘IO
- Redis支持持久化，可以将缓存数据保存在硬盘中，重启后可以恢复
- Redis支持多种数据类型和简单的事务，可以处理一些复杂的缓存场景
- Redis支持分布式和哨兵机制，可以实现高可用和负载均衡



### 5、为什么要用 Redis 而不用 map/guava 做缓存

- Redis可以存储几十个G的数据，而map/guava受限于JVM的内存大小
- Redis可以持久化数据，而map/guava是内存对象，程序重启后数据就丢失
- Redis可以实现分布式缓存，而map/guava只能存在于创建它的程序里
- Redis可以处理每秒百万级的并发，是专业的缓存服务



### 6、redis常见的应用场景有哪些

- 缓存：利用Redis的高速读写和过期策略，可以缓存一些热点数据，提高系统的响应速度和并发能力。
- 排行榜：利用Redis的有序集合（sorted set）数据类型，可以实现各种排行榜功能，如热门商品、热门文章、积分排名等。
- 计数器：利用Redis的原子操作和位图（bitmap）数据类型，可以实现各种计数器功能，如在线用户数、网站访问量、用户签到等。
- 分布式锁：利用Redis的setnx命令和过期时间，可以实现分布式锁功能，解决多个进程或服务器对同一资源的并发访问问题。
- 消息队列：利用Redis的列表（list）数据类型和阻塞弹出命令（blpop/brpop），可以实现消息队列功能，解决生产者和消费者之间的异步通信问题



### 7、redis有哪些数据类型

- string（字符串）：最基本的数据类型，可以存储任何二进制数据，如文本、图片、对象等，最大能存储512MB。
- hash（哈希）：类似于Java中的Map，可以存储一个对象的多个字段和值，适合用于存储用户信息、商品信息等。
- list（列表）：类似于Java中的LinkedList，可以在头尾两端进行插入和删除操作，适合用于实现栈、队列、消息列表等。
- set（集合）：类似于Java中的HashSet，可以存储多个不重复的元素，支持交集、并集、差集等操作，适合用于实现标签、好友关系等。
- zset（有序集合）：类似于Java中的TreeSet，可以存储多个不重复的元素，并给每个元素赋予一个分数（score），根据分数进行排序，适合用于实现排行榜、延时队列等。
- Bitmap：位图，Bitmap想象成一个以位为单位数组，数组中的每个单元只能存0或者1，数组的下标在Bitmap中叫做偏移量。使用Bitmap实现统计功能，更省空间。如果只需要统计数据的二值状态，例如商品有没有、用户在不在等，就可以使用 Bitmap，因为它只用一个 bit 位就能表示 0 或 1。
- Hyperloglog。HyperLogLog 是一种用于统计基数的数据集合类型，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。场景：统计网页的UV（即Unique Visitor，不重复访客，一个人访问某个网站多次，但是还是只计算为一次）。要注意，HyperLogLog 的统计规则是基于概率完成的，所以它给出的统计结果是有一定误差的，标准误算率是 0.81%。
- Geospatial ：主要用于存储地理位置信息，并对存储的信息进行操作，适用场景如朋友的定位、附近的人、打车距离计算等。



### 8、Redis持久化机制

Redis提供了两种不同的持久化方法可以将数据存储在磁盘中，一种叫快照`RDB`，另一种是`AOF`



RDB是一种使用快照方式的持久化，它会把当前的数据像拍照一样照下来，生成RDB文件保存到硬盘，是一个整个的形式。

AOF是一种日志式的持久化，每一条更新都会写入AOF文件，记录了每一个命令的详细信息.



RDB的优点：
- RDB文件是紧凑的二进制文件，比较适合做冷备，全量复制的场景。
- RDB文件可以在特定的时间间隔内生成，减少数据丢失的风险。
- RDB使用子进程进行持久化，对Redis服务进程性能影响较小。
- RDB恢复数据比较快，因为只需要加载一个文件。

RDB的缺点：
- RDB可能会丢失最后一次快照之后的数据。
- RDB在生成快照时，如果数据量太大，会消耗大量的内存和CPU资源。
- RDB不适合频繁备份的场景，因为每次都要生成一个完整的文件。

AOF的优点：
- AOF可以记录每一条写命令，保证数据的完整性和一致性。
- AOF可以根据不同的策略进行同步或异步写入，提高性能或安全性。
- AOF可以定期进行重写，减少文件大小和恢复时间。

AOF的缺点：
- AOF文件比RDB文件大，占用更多的磁盘空间。
- AOF恢复数据比较慢，因为需要重新执行所有的命令。
- AOF可能会遇到写入失败或文件损坏等问题。



### 9、RDB和AOF可以同时使用吗

RDB和AOF可以同时使用，但是会增加磁盘空间和IO开销。

如果同时使用RDB和AOF，Redis会优先使用AOF文件来还原数据，因为AOF文件更完整和一致。

Redis 4.0引入了一种混合持久化的功能，可以在AOF文件中包含RDB的快照，这样可以减少文件大小和恢复时间。



### 10、如何配置混合持久化

在Redis配置文件中设置两个选项：

- appendonly yes
- aof-use-rdb-preamble yes

这样，当AOF重写时，就会在AOF文件开头加上RDB的快照内容。你也可以通过命令行来设置这些选项，但是重启Redis服务后会失效



### 11、如何选择合适的持久化方式

选择合适的持久化方式要根据你的数据安全性和性能需求来决定

- 如果你需要最高级别的数据安全性，你可以同时使用RDB和AOF，这样可以保证在任何情况下都不会丢失数据。
- 如果你可以容忍一定程度的数据丢失，你可以只使用RDB或者AOF，并根据你的硬盘性能和恢复时间来调整保存频率。
- 如果你对数据安全性要求不高，你可以关闭持久化功能，这样可以获得最高的性能和最低的资源消耗。



### 12、混合持久化有哪些优缺点

优点

- 结合了RDB和AOF的优点，既可以快速启动，又可以减少数据丢失。
- AOF文件的大小和恢复时间都会减少，因为RDB快照占用了大部分空间。

缺点

- AOF文件的可读性变得很差，因为前半段是RDB格式的二进制数据。
- 兼容性差，如果开启混合持久化，那么这个AOF文件就不能用在Redis 4.0之前的版本了。



### 13、Redis持久化数据和缓存怎么做扩容

Redis持久化数据和缓存的扩容方法取决于你的使用场景¹²：

- 如果你把Redis当做缓存使用，你可以使用一致性哈希算法来实现动态扩容缩容，这样可以保证数据的均匀分布和最小迁移。
- 如果你把Redis当做持久化存储使用，你可以使用固定的keys-to-nodes映射关系来确定节点的数量，这样可以避免数据丢失和不一致。如果你需要动态变化节点的数量，你可以使用Redis Cluster或者其他支持数据再平衡的方案。



### 14、什么是一致性哈希

一致性哈希算法是一种特殊的哈希算法，它可以在分布式缓存中实现动态扩容缩容¹²。

它的原理是把所有的服务器和数据都映射到一个环形空间上，然后根据数据的哈希值找到对应的服务器。当增加或删除一个服务器时，只需要重新分配该服务器负责的一小部分数据，而不影响其他服务器和数据³⁴⁵。这样可以保证数据的均匀分布和最小迁移

一致性哈希算法的优点是可以有效地解决分布式缓存中的动态扩容缩容问题，保证数据的均匀分布和最小迁移

缺点是可能造成数据倾斜¹，不支持自定义的槽位标号²，不灵活³，不能保证流量和负载的均匀



### 15、Redis过期键的删除策略

**定时删除**

在设置某个key 的过期时间同时，我们创建一个定时器，让定时器在该过期时间到来时，立即执行对其进行删除的操作。

优点：定时删除对内存是最友好的，能够保存内存的key一旦过期就能立即从内存中删除。

缺点：对CPU最不友好，在过期键比较多的时候，删除过期键会占用一部分 CPU 时间，对服务器的响应时间和吞吐量造成影响。

**惰性删除**

设置该key 过期时间后，我们不去管它，当需要该key时，我们在检查其是否过期，如果过期，我们就删掉它，反之返回该key。

优点：对 CPU友好，我们只会在使用该键时才会进行过期检查，对于很多用不到的key不用浪费时间进行过期检查。

缺点：对内存不友好，如果一个键已经过期，但是一直没有使用，那么该键就会一直存在内存中，如果数据库中有很多这种使用不到的过期键，这些键便永远不会被删除，内存永远不会释放。从而造成内存泄漏。

**定期删除**

每隔一段时间，我们就对一些key进行检查，删除里面过期的key。

优点：可以通过限制删除操作执行的时长和频率来减少删除操作对 CPU 的影响。另外定期删除，也能有效释放过期键占用的内存。

缺点：难以确定删除操作执行的时长和频率。如果执行的太频繁，定期删除策略变得和定时删除策略一样，对CPU不友好。如果执行的太少，那又和惰性删除一样了，过期键占用的内存不会及时得到释放。另外最重要的是，在获取某个键时，如果某个键的过期时间已经到了，但是还没执行定期删除，那么就会返回这个键的值，这是业务不能忍受的错误。

**Redis的过期删除策略就是：惰性删除和定期删除两种策略配合使用。**



### 16、Redis key的过期时间和永久有效分别怎么设置？

通过expire或pexpire命令，客户端可以以秒或毫秒的精度为数据库中的某个键设置生存时间。

与expire和pexpire命令类似，客户端可以通过expireat和pexpireat命令，以秒或毫秒精度给数据库中的某个键设置过期时间，可以理解为：让某个键在某个时间点过期。



### 17、redis的内存淘汰策略

你好，这是Bing。根据网络搜索结果，Redis内存淘汰策略是指当内存使用达到maxmemory极限时，需要使用不同的算法来决定清理掉哪些数据，以保证新数据的存入

目前有八种淘汰策略

- noeviction：当内存超出限制时，不会驱逐任何键，而是返回错误。
- allkeys-random：从所有键中随机删除。
- allkeys-lru：从所有键中删除最近最少使用的键。
- allkeys-lfu：从所有键中删除最近最不频繁使用的键。
- volatile-random：从设置了过期时间的键中随机删除。
- volatile-lru：从设置了过期时间的键中删除最近最少使用的键。
- volatile-lfu：从设置了过期时间的键中删除最近最不频繁使用的键。
- volatile-ttl：从设置了过期时间的键中删除即将过期的键。



### 18、 如何保证缓存与数据库双写时的数据一致性？

共有四种方案：

1. 先更新数据库，后更新缓存
2. 先更新缓存，后更新数据库
3. 先删除缓存，后更新数据库
4. 先更新数据库，后删除缓存

第一种方案：并发更新数据库场景下，会将脏数据刷到缓存。

第二种方案：如果先更新缓存成功，但是数据库更新失败，则肯定会造成数据不一致。

第三种方案：删除缓存后，在更新数据库前有可能被其它的线程将数据库的旧值重新读取到缓存

第四种方案：更新数据库后删除缓存失败时导致读取错误值；



第三种方案完善：延时双删，将更新时脏数据再次删除；更新与读取操作进行异步串行化，使用队列存储操作，当更新数据库时，新的查询请求发现缓存为空，先检查队列里有没有更新的操作，有的话先等待。



第四种方案完善：将Redis 的 key 作为消息体发送到消息队列中，系统接收到消息队列发送的消息后再次对 Redis 进行删除操作



### 19、什么是缓存击穿

缓存击穿是指使用不存在的key进行大量的高并发查询，这导致缓存无法命中，每次请求都要击穿到后端数据库系统进行查询，使数据库压力过大，甚至使数据库服务被压死¹。缓存击穿通常是由恶意攻击或者无意造成的²。

解决方案

+ 使用互斥锁，只有拿到锁才可以查询数据库，降低了在同一时刻打在数据库上的请求³。

- 对于热点的key可以设置永不过期的key，这样就避免了缓存失效的情况。
- 使用job给指定key自动续期，这样就能保证缓存不会失效。
- 使用布隆过滤器或者其他数据结构来过滤掉不存在的key，这样就减少了无效的查询。



### 20、什么是缓存穿透

缓存穿透是指查询一个一定不存在的数据，由于缓存是不命中时被动写的，并且出于容错考虑，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到存储层去查询，这样会给数据库造成巨大的压力和冲击

解决方案

- 将无效的key存放进Redis中：

当出现Redis查不到数据，数据库也查不到数据的情况，我们就把这个key保存到Redis中，设置value="null"，并设置其过期时间极短，后面再出现查询这个key的请求的时候，直接返回null，就不需要再查询数据库了。

- 使用布隆过滤器：

如果布隆过滤器判定某个 key 不存在布隆过滤器中，那么就一定不存在，如果判定某个 key 存在，那么很大可能是存在(存在一定的误判率)。于是我们可以在缓存之前再加一个布隆过滤器，将数据库中的所有key都存储在布隆过滤器中，在查询Redis前先去布隆过滤器查询 key 是否存在，如果不存在就直接返回，不让其访问数据库，从而避免了对底层存储系统的查询压力。



### 21、什么是缓存雪崩

缓存雪崩是指缓存层由于某些原因（比如宕机、cache服务挂了或者缓存集中过期）整体crash掉了，导致大量请求到达后端数据库，从而导致数据库崩溃

解决方案

解决缓存雪崩的一般思路是尽量避免缓存同时失效或者缓存层不可用的情况。具体的方案有以下几种¹²³⁴：
- 给缓存的失效时间加上一个随机值，避免集体失效。
- 分级缓存，设置多级缓存，或者备用缓存
- 热点数据缓存永远不过期
- 保证Redis的高可用，部署成高可用集群，防止缓存层宕机。
- 使用限流降级的方式，当流量到达一定阈值时，直接返回提示或者默认值，减轻数据库压力。
- 结合业务特点和场景，优化缓存策略和数据结构



### 22、 什么是缓存预热

缓存预热是指系统上线后，提前将相关的缓存数据加载到缓存系统

- 数据量不大的时候，工程启动的时候进行加载缓存动作；
- 数据量大的时候，设置一个定时任务脚本，进行缓存的刷新；
- 数据量太大的时候，优先保证热点数据进行提前加载到缓存



### 23、什么是缓存降级

缓存降级是指在缓存失效或者缓存服务出现问题时，为了保证主服务的可用性，不去访问数据库或者下一级缓存，而是直接返回默认数据或者内存数据



### 24、介绍一下布隆过滤器

布隆过滤器是一种高空间利用率的概率性数据结构，由二进制向量和一系列随机映射函数组成¹。

它可以用来判断一个元素是否在一个集合中，但是有一定的误识别率⁴。

原理

首先分配一块二进制向量（位数组），默认都是0。然后使用k个不同的哈希函数，对每个元素进行哈希计算，得到k个位置，并将这些位置都设为1。当要查询一个元素是否存在时，也用同样的哈希函数计算出k个位置，如果这些位置都是1，则认为该元素可能存在；如果有任何一个位置是0，则认为该元素一定不存在



### 25、 Redis为何选择单线程

Redis是一个基于内存的键值对数据库，它的网络IO和键值对读写是由一个线程来完成的，这就是所谓的“单线程”。Redis选择单线程的原因有以下几点³⁴：
- 避免过多的上下文切换开销：如果是单线程则可以规避进程内频繁的线程切换开销。
- 避免同步机制的开销：如果Redis选择多线程模型，又因为Redis是共享内存模型，那么就需要使用锁机制来保证数据一致性，这会带来额外的性能损耗。
- 利用CPU缓存：如果Redis使用多线程模型，那么不同的线程可能会访问不同的CPU核心，导致CPU缓存失效，而单线程则可以充分利用CPU缓存。
- Redis本身处理速度很快：Redis是基于内存操作的数据库，它使用了高效的数据结构和算法来实现各种命令，所以它本身处理速度很快，不需要依赖多线程来提升性能



### 26、Redis 6.0为何引入多线程

- 可以充分利用CPU多核，提高并发性能。Redis 6.0之前的主线程只能利用一个核，而多线程可以让不同的线程访问不同的CPU核心。
- 可以分摊Redis同步IO读写负荷。Redis 6.0之前的主线程需要负责接收、解析、执行和返回客户端的请求，而多线程可以将网络IO和协议解析的任务交给IO线程处理，减轻主线程的压力
- Redis 6 引入的多线程 IO 特性对性能提升至少是一倍以上



**线程并发安全问题**

Redis 的多线程部分只是用来处理网络数据的读写和协议解析，执行命令仍然是单线程顺序执行



### 27、介绍下Redis的线程模型

Redis的线程模型分为两种：单线程模型和多线程模型。
- 单线程模型是Redis的传统模式，它使用一个主线程来处理客户端的请求，包括接收、解析、执行和返回。主线程使用IO多路复用机制来同时监听多个socket，并根据socket上的事件来选择对应的事件处理器进行处理²³。这种模式的优点是简单高效，能保证指令的顺序执行，缺点是不能充分利用CPU多核资源，也不能分摊同步IO读写负荷。
- 多线程模型是Redis 6.0引入的新特性，它使用一组IO线程来辅助主线程处理网络数据的读写和协议解析，而主线程仍然负责执行命令和返回结果。主线程在处理完读事件后，通过Round Robin将这些连接分配给IO线程，然后阻塞等待IO读取socket完毕¹⁴⁶。这种模式的优点是可以提高并发性能，利用CPU多核资源，也可以减轻主线程的压力，缺点是增加了复杂度和开销，也可能导致数据不一致或乱序。



### 28、说一下redis的事务

Redis事务是一种将多个命令打包，然后一次性、按顺序地执行的机制¹。Redis事务有以下特点：
- Redis事务中的命令不会被其他客户端的命令打断，但也不会阻塞其他客户端的连接¹。
- Redis事务中的命令在执行时可能会失败，但不会影响其他命令的执行，也不会回滚已执行的命令³。
- Redis事务使用MULTI、EXEC、DISCARD等命令来控制事务的开始、结束和取消

Redis使用WATCH命令来决定事务是继续执行还是回滚，那就需要在MULTI之前使用WATCH来监控某些键值对，然后使用MULTI命令来开启事务，执行对数据结构操作的各种命令，此时这些命令入队列。

当使用EXEC执行事务时，首先会比对WATCH所监控的键值对，如果没发生改变，它会执行事务队列中的命令，提交事务；如果发生变化，将不会执行事务中的任何命令，同时事务回滚。



### 29、说一下redis事务和lua脚本的区别

Lua脚本和事务的区别主要有以下几点：
- 实现逻辑：Lua脚本是基于Redis的单线程执行命令，而事务是基于乐观锁。
- 自定义逻辑：Lua脚本可以加入自定义逻辑，比如条件判断、循环等，而事务只能一次命令的批量执行。
- 阻塞性：Lua脚本在执行时会阻塞其他客户端的命令，所以不宜写太耗时的处理逻辑，而事务不会。



### 30、说一下redis常用搭建方式

- Redis单副本；
- Redis多副本（主从）；
- Redis Sentinel（哨兵）；
- Redis Cluster；
- Redis自研。

使用场景：

如果数据量很少，主要是承载高并发高性能的场景，比如缓存一般就几个G的话，单机足够了。

主从模式：master 节点挂掉后，需要手动指定新的 master，可用性不高，基本不用。

哨兵模式：master 节点挂掉后，哨兵进程会主动选举新的 master，可用性高，但是每个节点存储的数据是一样的，浪费内存空间。数据量不是很多，集群规模不是很大，需要自动容错容灾的时候使用。

Redis cluster 主要是针对海量数据+高并发+高可用的场景，如果是海量数据，如果你的数据量很大，那么建议就用Redis cluster，所有master的容量总和就是Redis cluster可缓存的数据容量。



### 31、redis介绍下Redis单副本

采用单个Redis节点部署架构，没有备用节点实时同步数据，不提供数据持久化和备份策略，适用于数据可靠性要求不高的纯缓存业务场景。

**优点：**

- 架构简单，部署方便；
- 高性价比：缓存使用时无需备用节点（单实例可用性可以用supervisor或crontab保证），当然为了满足业务的高可用性，也可以牺牲一个备用节点，但同时刻只有一个实例对外提供服务；
- 高性能。

**缺点：**

- 不保证数据的可靠性；
- 在缓存使用，进程重启后，数据丢失，即使有备用的节点解决高可用性，但是仍然不能解决缓存预热问题，因此不适用于数据可靠性要求高的业务；
- 高性能受限于单核CPU的处理能力（Redis是单线程机制），CPU为主要瓶颈，所以适合操作命令简单，排序、计算较少的场景。也可以考虑用Memcached替代。



### 32、介绍下主从复制

Redis多副本，采用主从（replication）部署结构，相较于单副本而言最大的特点就是主从实例间数据实时同步，并且提供数据持久化和备份策略。主从实例部署在不同的物理服务器上，根据公司的基础环境配置，可以实现同时对外提供服务和读写分离策略。

- 优点：
  - 可以实现数据的备份，提高数据的安全性。²³
  - 可以实现读写分离，提高数据的吞吐量。²³
  - 可以实现负载均衡，降低主服务器的压力。³
  - 主服务器是非阻塞的，可以在同步期间继续处理客户端请求。³
- 缺点：
  - 数据一致性不能保证，因为主从复制是异步的，可能会出现主从数据不一致的情况。²⁴
  - 主服务器宕机后，需要手动切换从服务器为主服务器，或者使用哨兵机制来自动切换。⁴
  - 复制过程中可能会产生网络延迟或者丢包等问题，影响复制效率和准确性。⁴





### 33、介绍下哨兵机制

Redis哨兵机制是一种高可用性的解决方案，它可以监控主从节点的状态，并在主节点故障时自动切换从节点为新的主节点。¹²³

Redis Sentinel的节点数量要满足2n+1（n>=1）的奇数个。

- 哨兵机制由一个或多个哨兵实例组成，每个哨兵实例都会定期向主从节点发送心跳包，检测节点的可达性和角色。¹²⁴
- 当一个哨兵实例发现主节点不可达时，它会向其他哨兵实例发送询问消息，如果超过一定数量的哨兵实例都认为主节点不可达，那么这个主节点就被判定为下线。²⁴
- 当主节点下线后，哨兵系统会选举出一个领头哨兵，负责执行故障转移操作。领头哨兵会根据一些优先级规则选择一个从节点作为新的主节点，并向其发送命令让其升级为主节点。²⁴
- 故障转移完成后，领头哨兵会通知其他从节点和客户端新的主节点信息，并重新配置他们与新的主节点建立连接
- 原来的主节点上线后，让他成为从结点



被选举为master的标准是什么？

- 跟master断开连接的时长。 如果一个slave跟master断开连接已经超过了down-after-milliseconds的10倍，外加master宕机的时长，那么slave就被认为不适合选举为master.

```
( down-after-milliseconds * 10) + milliseconds_since_master_is_in_SDOWN_state
```

- slave优先级。 按照slave优先级进行排序，slave priority越低，优先级就越高
- 复制offset。 如果slave priority相同，那么看replica offset，哪个slave复制了越多的数据，offset越靠后，优先级就越高
- run id 如果上面两个条件都相同，那么选择一个run id比较小的那个slave。



### 34、查看哨兵状态

查看Redis哨兵状态有以下几种方法：¹²³

- 使用`info`命令，可以查看哨兵实例的一些基本信息，如运行模式、版本、角色、监控的主节点等。¹²
- 使用`sentinel masters`命令，可以查看哨兵系统监控的所有主节点的信息，如名称、地址、状态、从节点数量等。¹²
- 使用`sentinel slaves <master-name>`命令，可以查看指定主节点的所有从节点的信息，如名称、地址、状态、优先级等。¹²
- 使用`sentinel sentinels <master-name>`命令，可以查看指定主节点的所有其他哨兵实例的信息，如名称、地址、状态等。¹²



### 35、如何测试Redis故障转移

- 使用`kill -9`命令，模拟主节点的宕机，观察哨兵系统是否能够自动选举新的主节点，并通知其他从节点和客户端。²
- 使用`redis-cli -p <port> debug sleep <seconds>`命令，模拟主节点的卡顿，观察哨兵系统是否能够在超时时间内切换新的主节点，并恢复原来的主节点为从节点。²
- 使用`redis-cli -p <port> cluster failover`命令，手动触发故障转移，让当前的主节点变为从节点，并让一个从节点变为新的主节点。¹



### 36、介绍下redis集群

Redis Cluster集群节点最小配置6个节点以上（3主3从），其中主节点提供读写操作，从节点作为备用节点，不提供请求，只作为故障转移使用。

Redis Cluster采用虚拟槽分区，所有的键根据哈希函数映射到0～16383个整数槽内，每个节点负责维护一部分槽以及槽所印映射的键值数据。

Redis Cluster是一种分布式的Redis实现，它具有以下特点：¹²

- 高性能和线性可扩展性，最多支持1000个节点。¹
- 无需代理，使用异步复制，不对值进行合并操作。¹
- 自动分片，将数据按照哈希槽分配到不同的节点上。
- 容错性，当部分节点故障时，仍能提供服务。¹

  

### 37、redis高可用有哪些方案

Redis高可用方案具体怎么实施，取决于你的业务场景和需求。一般来说，有以下几种常见的方案：¹²³⁴⁵

- 数据持久化，通过RDB或AOF方式将内存中的数据保存到磁盘上，防止数据丢失。¹
- 主从同步（主从复制），通过配置一个主节点和多个从节点，让从节点定时复制主节点的数据，实现读写分离和负载均衡。¹⁴
- 哨兵模式（Sentinel），通过配置多个哨兵节点，监控主从节点的运行状态，当主节点故障时，自动选举新的主节点，并通知其他从节点和客户端。¹²⁴
- 集群模式（Cluster），通过配置多个主从组成的分片（shard），将数据按照哈希槽分配到不同的分片上，实现水平扩展和容错能力。



一般来说，主从同步方案可以提高读取性能，但是会增加写入延迟和数据不一致的风险。 

哨兵模式可以提高可用性，但是会增加配置复杂度和故障转移时间。

集群模式可以提高扩展性和容错性，但是会增加网络开销和数据分片的限制。

你需要根据你的业务需求和预期效果，权衡各种方案的利弊，选择最适合你的方案。



### 38、了解redis主从复制的原理吗

主从复制的过程分为三个阶段：³⁴
- 同步：从服务器向主服务器发送SYNC命令，主服务器开始执行BGSAVE命令，生成RDB文件，并将写命令缓存起来。
- 命令传播：主服务器将RDB文件发送给从服务器，从服务器接收并加载RDB文件，然后向主服务器发送ACK命令。
- 增量复制：主服务器将缓存的写命令依次发送给从服务器，从服务器执行这些写命令，保持和主服务器的数据一致
- 主从复制的断点续传
- 无磁盘化复制：master在内存中直接创建rdb，然后发送给slave，不会在自己本地落地磁盘
- 过期key处理：slave不会过期key，只会等待master过期key。如果master过期了一个key，或者通过LRU淘汰了一个key，那么会模拟一条del命令发送给slave




### 39、由于主从延迟导致读取到过期数据怎么处理？

- 在业务层面，可以设置缓存的有效期比Redis中的过期时间短，这样可以避免读取到过期数据。
- 在从节点层面，可以修改slave-serve-stale-data参数为no，这样当从节点处于复制同步过程中或者复制延迟超过阈值时，就不会响应客户端的请求。



### 40、Redis主从架构数据会丢失吗

Redis主从架构数据会丢失的可能性是存在的，主要有两种情况：¹²
- 异步复制导致数据丢失。因为主从库间的命令复制是异步进行的，所以有可能在主库宕机或网络断开时，部分数据还没有来得及复制到从库上，导致数据丢失。
- 集群脑裂导致数据丢失。因为哨兵机制是基于投票机制来选举新的主库的，所以有可能在网络分区时，出现多个主库同时提供服务的情况，导致数据不一致。

为了防止数据丢失，可以采取一些措施：³
- 使用Redis持久化功能，将内存中的数据定期保存到磁盘上，作为备份和恢复。
- 使用Redis同步复制功能，在主库执行写命令前等待至少一个从库确认收到命令。
- 使用Redis集群模式，实现分片和高可用。



### 41、如何解决脑裂数据丢问题

防止脑裂发生的一种常见方法是使用仲裁或隔离机制，即在两个节点出现分歧时，由第三方的仲裁者决定听谁的²³。例如，Redis提供了两个配置项来限制主库的请求处理，分别是 min-slaves-to-write 和 min-slaves-max-lag¹。这样可以确保主库只有在能够和足够数量的从库进行数据同步时才接收客户端请求。



### 42、同步配置的时候其他哨兵根据什么更新自己的配置呢

同步配置的时候其他哨兵根据**版本号**更新自己的配置⁴。执行切换的那个哨兵，会从要切换到的新master那里得到一个configuration epoch，这就是一个version号，每次切换的version号都必须是唯一的。这个version号就很重要了，因为各种消息都是通过一个channel去发布和监听的，所以一个哨兵完成一次新的切换之后，新的master配置是跟着新的version号的，其他的哨兵都是根据版本号的大小来更新自己的master配置⁴。



### 43、为什么Redis哨兵集群只有2个节点无法正常工作？

 Redis哨兵集群只有2个节点无法正常工作的原因是**缺少多数派**¹²。如果master宕机，两个哨兵中只要有一个认为master宕机就可以执行故障转移，但是如果其中一个哨兵也宕机了，那么剩下的一个哨兵没有办法执行故障转移，因为它需要得到多数派的同意¹²³。所以至少需要三个哨兵才能保证高可用性³。



### 44、Redis cluster中是如何实现数据分布的

根据我搜索到的信息，Redis cluster中是通过**虚拟槽分区**实现数据分布的¹²³。虚拟槽分区使用了哈希空间，把所有数据映射到一个固定范围的整数集合中，这个整数定义为槽（slot），而且这个槽的个数一般远远的大于节点数。每个节点负责一部分槽，每个槽对应一部分数据。当需要访问一个键值对时，先计算它属于哪个槽，再找到负责这个槽的节点¹²³。



### 45、Redis cluster节点间通信是什么机制

Redis cluster中节点之间是通过**gossip协议**进行通信的¹²³。gossip协议是一种P2P的方式，节点彼此不断通信交换信息，一段时间后所有的节点都会知道集群完整的信息²⁴。每个节点都要开放两个端口号，一个是用来提供服务的，另一个是用来进行节点间通信的¹²。



### 46、说一下分布式锁

使用分布式锁的目的是为了保证在分布式系统中，多个进程对同一个共享资源的操作是互斥的，避免数据的不一致或者重复执行²³⁴。例如，如果有多个批处理任务需要执行，但是每个任务只能被一个进程领取和处理，那么就需要使用分布式锁来控制任务的领取和执行



选择合适的分布式锁实现方式需要考虑以下几个方面¹²³：

- 锁的可靠性：锁是否能够保证互斥性，是否能够避免死锁，是否能够在异常情况下自动释放。
- 锁的性能：锁的获取和释放的时间开销，锁的存储和传输的空间开销，锁对系统资源的消耗。
- 锁的易用性：锁的实现和使用是否简单明了，是否有成熟的框架或工具支持。

根据不同的场景和需求，可以选择基于ZooKeeper、Redis、数据库等不同方式来实现分布式锁¹²³。每种方式都有其优缺点，例如：

- 基于ZooKeeper的分布式锁可以保证高可靠性和强一致性，但是性能较低，且需要额外部署ZooKeeper集群。
- 基于Redis的分布式锁可以提供高性能和高可用性，但是需要注意设置合理的过期时间和续约机制，以及处理网络分区问题。
- 基于数据库的分布式锁可以利用已有的数据库资源，实现简单，但是对数据库压力较大，且可能存在死锁风险。



### 47、redis怎么实现分布式锁

基于Redis实现分布式锁的一种常用方法是使用SET命令带上NX和EX选项¹²³。NX选项表示只有当key不存在时才设置值，EX选项表示设置key的过期时间。这样可以保证加锁和设置过期时间是原子操作，避免了锁无法释放的风险。同时，为了防止误释放别人的锁，每个客户端需要设置一个唯一的值作为锁的标识¹²³。



缺点

1、SETNX 和 EXPIRE 非原子性

假设一个场景中，某一个线程刚执行setnx，成功得到了锁。此时setnx刚执行成功，还未来得及执行expire命令，节点就挂掉了。此时这把锁就没有设置过期时间，别的线程就再也无法获得该锁。

**解决措施:**

由于`setnx`指令本身是不支持传入超时时间的，而在Redis2.6.12版本上为`set`指令增加了可选参数, 用法如下：

```
SET key value [EX seconds][PX milliseconds] [NX|XX]
```

- EX second: 设置键的过期时间为second秒；
- PX millisecond：设置键的过期时间为millisecond毫秒；
- NX：只在键不存在时，才对键进行设置操作；
- XX：只在键已经存在时，才对键进行设置操作；
- SET操作完成时，返回OK，否则返回nil。

2、锁误解除

如果线程 A 成功获取到了锁，并且设置了过期时间 30 秒，但线程 A 执行时间超过了 30 秒，锁过期自动释放，此时线程 B 获取到了锁；随后 A 执行完成，线程 A 使用 DEL 命令来释放锁，但此时线程 B 加的锁还没有执行完成，线程 A 实际释放的线程 B 加的锁。

**解决办法：**

在del释放锁之前加一个判断，验证当前的锁是不是自己加的锁。

具体在加锁的时候把当前线程的id当做value，可生成一个 UUID 标识当前线程，在删除之前验证key对应的value是不是自己线程的id。

3、超时解锁导致并发

如果线程 A 成功获取锁并设置过期时间 30 秒，但线程 A 执行时间超过了 30 秒，锁过期自动释放，此时线程 B 获取到了锁，线程 A 和线程 B 并发执行。

A、B 两个线程发生并发显然是不被允许的，一般有两种方式解决该问题：

- 将过期时间设置足够长，确保代码逻辑在锁释放之前能够执行完成。
- 为获取锁的线程增加守护线程，为将要过期但未释放的锁增加有效时间。

4、不可重入

当线程在持有锁的情况下再次请求加锁，如果一个锁支持一个线程多次加锁，那么这个锁就是可重入的。如果一个不可重入锁被再次加锁，由于该锁已经被持有，再次加锁会失败。Redis 可通过对锁进行重入计数，加锁时加 1，解锁时减 1，当计数归 0 时释放锁。

5、无法等待锁释放

上述命令执行都是立即返回的，客户端正常来讲不会阻塞等待锁释放

- 可以通过客户端轮询的方式解决该问题，当未获取到锁时，等待一段时间重新获取锁，直到成功获取锁或等待超时。这种方式比较消耗服务器资源，当并发量比较大时，会影响服务器的效率。
- 另一种方式是使用 Redis 的发布订阅功能，当获取锁失败时，订阅锁释放消息，获取锁成功后释放时，发送锁释放消息。



### 48、了解RedLock吗

RedLock是一种分布式锁管理器，它允许多个进程在多个服务器上协调访问分布式环境中的共享资源¹²。

它可以确保任何时候只有一个进程可以持有锁，而且即使在网络故障或其他异常情况下，锁也可以被释放²



RedLock算法的基本步骤如下³：

- 客户端获取当前时间戳。
- 客户端依次向N个Redis主节点发送一个带有随机值和过期时间的SETNX命令，尝试获取锁。
- 客户端检查是否至少有N/2+1个Redis节点成功设置了锁，并且总的延迟时间没有超过过期时间的一半。
- 如果成功获取锁，客户端返回一个由随机值和有效期组成的锁对象；否则，客户端向所有Redis节点发送DEL命令释放可能设置的锁，并返回失败。
- 当客户端不再需要锁时，它会向所有Redis节点发送一个带有随机值检查的DEL命令释放锁。如果随机值不匹配，则说明锁已经被其他客户端获取或释放，此时不应该删除锁。



RedLock算法的优点有：
- 它比单个Redis实例的锁更安全，因为它可以容忍一些Redis节点的故障或网络分区¹。
- 它可以在分布式系统中防止对共享资源的并发访问，避免数据损坏或不一致的结果²。
- 它可以在锁被意外释放或过期后，自动重试获取锁，减少等待时间和失败率²。



RedLock算法的缺点有：
- 它比单个Redis实例的锁更复杂和昂贵，因为它需要多个Redis节点和多次网络通信¹³。
- 它不能保证绝对的安全性，因为在极端情况下，可能会出现锁被多个客户端同时持有或过早释放的情况¹。
- 它对Redis节点的数量和延迟时间有一定的要求，如果不满足这些要求，可能会影响锁的正确性和可用性¹²。



RedLock算法适用于以下场景：
- 当你需要在分布式系统中对共享资源进行互斥访问，而且这些资源不是关键的或者可以容忍一定的错误率¹²。
- 当你需要在多个Redis节点之间实现一致性，而且这些节点是完全独立的，不使用复制或其他隐式协调系统¹³。
- 当你需要在锁被意外释放或过期后，自动重试获取锁，减少等待时间和失败率¹³。



RedLock算法处理网络故障的方式是：
- 当客户端向Redis节点发送SETNX命令时，如果没有收到响应，或者响应超时，客户端会认为该节点失败，并继续尝试其他节点¹²。
- 当客户端检查是否成功获取锁时，如果没有至少N/2+1个Redis节点成功设置了锁，或者总的延迟时间超过了过期时间的一半，客户端会认为获取锁失败，并向所有Redis节点发送DEL命令释放可能设置的锁¹²。
- 当客户端不再需要锁时，如果向某个Redis节点发送DEL命令失败，或者响应超时，客户端会忽略该节点，并继续向其他节点发送DEL命令释放锁¹²。



### 49、Redis如何做内存优化

- 使用合适的数据类型和编码，比如使用hash、list、set、zset代替string，使用ziplist或intset代替linkedlist或skiplist。
- 使用过期策略和淘汰策略，比如设置maxmemory参数和合理的淘汰算法（如volatile-lru）。
- 使用压缩功能，比如使用snappy压缩aof文件或rdb文件。
- 使用碎片整理功能，比如定期执行info memory命令检查mem_fragmentation_ratio值，如果大于1.5则重启Redis实例或使用redis-cli --rdb进行在线碎片整理



### 50、如果现在有个读超高并发的系统，用Redis来抗住大部分读请求，你会怎么设计？

如果是读高并发的话，先看读并发的数量级是多少，因为Redis单机的读QPS在万级，每秒几万没问题，使用一主多从+哨兵集群的缓存架构来承载每秒10W+的读并发，主从复制，读写分离。

使用哨兵集群主要是提高缓存架构的可用性，解决单点故障问题。主库负责写，多个从库负责读，支持水平扩容，根据读请求的QPS来决定加多少个Redis从实例。如果读并发继续增加的话，只需要增加Redis从实例就行了。

如果需要缓存1T+的数据，选择Redis cluster模式，每个主节点存一部分数据，假设一个master存32G，那只需要n*32G>=1T，n个这样的master节点就可以支持1T+的海量数据的存储了。

> Redis单主的瓶颈不在于读写的并发，而在于内存容量，即使是一主多从也是不能解决该问题，因为一主多从架构下，多个slave的数据和master的完全一样。假如master是10G那slave也只能存10G数据。所以数据量受单主的影响。 而这个时候又需要缓存海量数据，那就必须得有多主了，并且多个主保存的数据还不能一样。Redis官方给出的 Redis cluster 模式完美的解决了这个问题。





### 51、说一下redis底层的数据结构

**SDS**

- sds是Redis底层所使用的字符串表示，它是一个结构体，包含了长度、空闲空间和字节数组三个属性。
- sds可以动态地调整字符串的大小，避免缓冲区溢出，并且获取字符串长度的复杂度为O(1)。
- sds可以兼容C语言传统的字符串表示（以空字符结尾的字符数组），因为它在字节数组的末尾也添加了一个空字符。
- sds可以存储任意二进制数据，不仅仅是文本数据，因为它不依赖于空字符来判断字符串是否结束。
- sds可以减少修改字符串时带来的内存重分配次数，因为它会预分配一定量的未使用空间，并且根据修改后的字符串长度来决定是否释放多余的空间。



**quicklist**

- Redis的quicklist是一种数据结构，它是一个双向链表，每个节点是一个ziplist（压缩列表）。
- Redis的quicklist是在3.2版本后引入的，它取代了原来的ziplist和linkedlist（双端链表）作为列表类型的底层实现。
- Redis的quicklist可以节省内存空间，因为它可以对节点进行LZF压缩算法，从而减少节点占用的字节数。
- Redis的quicklist可以配置compress参数，表示从两端开始有多少个节点不进行压缩。compress为0表示所有节点都不压缩。



**跳表**

- Redis的跳表是一种有序的多层链表，它通过在每个节点中增加多个指向后续节点的指针，来实现快速查找、插入和删除操作。
- Redis使用跳表作为有序集合键的底层实现之一，当有序集合包含的元素数量比较多或者元素成员是比较长的字符串时，Redis就会使用跳表来节省空间和提高效率。
- Redis的跳表由zskiplistNode和zskiplist两个结构定义，其中zskiplistNode表示跳表节点，而zskiplist表示跳表本身，包含了节点数量、头尾指针等信息。
- Redis的跳表在创建和插入节点时，使用随机算法来决定节点的层数，并且保证每层链表中节点个数是下层链表中节点个数的1/4（P=0.25），这样可以使得查找过程类似于二分查找，时间复杂度为O(logN)。



**压缩列表**

- Redis的压缩列表是一种线性数据结构，它是一个字节数组，可以包含任意多个元素，每个元素可以是一个字节数组或一个整数。
- Redis的压缩列表是为了节约内存而设计的，它使用特殊的编码方式来减少每个元素占用的字节数。
- Redis的压缩列表由多个字段组成，包括zlbytes（压缩列表总长度）、zltail（尾元素偏移量）、zllen（元素个数）、entryX（各个元素）和zlend（结束标志）。
- Redis的压缩列表在创建新列表时默认使用，当列表中有超过一定长度或者数量的元素时，会转换成双向链表或者快速列表。



**listpack**

- Redis的listpack是一种字符串列表的序列化格式，它是Redis5.0引入的一个全新的数据结构，设计用来取代ziplist。
- Redis的listpack和ziplist类似，都是使用特殊的编码方式来压缩存储字符串或整数，但是listpack解决了ziplist存在的级联更新的问题。
- Redis的listpack由多个字段组成，包括tot-bytes（总字节数）、num-elements（元素个数）和element-X（各个元素），每个元素都记录自己的长度，并放在节点尾部。
- Redis目前使用listpack作为t_hash和stream中radix tree节点中存储前缀的底层结构。



**整数集合**

- 整数集合（intset）是Redis用于保存整数值的集合抽象数据结构，它可以保存类型为int16_t、int32_t或者int64_t的整数值，并且保证集合中不会出现重复元素。
- 整数集合是集合键的底层实现之一，当一个集合只包含整数值元素，并且这个集合的元素数量不多时，Redis就会使用整数集合作为集合键的底层实现。
- 整数集合由一个intset结构和一个contents数组组成，其中intset结构保存了编码方式、元素数量等信息，而contents数组按照从小到大的顺序保存了所有元素。
- 整数集合在添加或删除元素时，会根据需要动态地调整contents数组的大小和编码方式，以节省空间和提高效率。



**哈希表**

- Redis的哈希表是一种数据类型，它是一个字符串类型的字段和值的映射表，特别适合用于存储对象。
- Redis中每个哈希表可以存储2 32 - 1个键值对（40多亿）。
- Redis提供了多种命令来操作哈希表，例如HMSET、HGET、HDEL等。
- Redis的哈希表使用字典作为底层实现，字典是一个包含两个哈希表的结构体，其中一个用于正常存储数据，另一个用于扩展或收缩空间时进行数据迁移。
- Redis的哈希表使用链地址法来解决键冲突问题，并且使用随机数种子和乘法散列算法来计算键的索引位置。



### 52、说一下渐进式哈希

- 渐进式哈希是Redis用于扩展或收缩哈希表的一种机制，它可以将rehash键值对所需的计算工作均摊到对字典的每个添加、删除、查找和更新操作上，从而避免了集中式rehash而带来的庞大计算量。
- 渐进式哈希的过程是这样的：当哈希表需要扩容或收缩时，Redis会为ht[1]分配空间，并将rehashidx设置为0，表示开始rehash。然后，在每次对字典进行操作时，都会从ht[0]中取出一部分键值对，根据新的哈希函数计算它们在ht[1]中的位置，并将它们添加到ht[1]中。同时，rehashidx会递增，表示已经完成了一部分rehash工作。
- 当ht[0]中所有键值对都被移动到ht[1]中时，表示rehash完成。此时，释放ht[0]占用的空间，并将ht赋值为ht[1]。最后，将rehashidx设置为-1，表示字典不再处于rehash状态。