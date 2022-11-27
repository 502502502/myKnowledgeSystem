[TOC]



# 一、Hibernate简介

## 1、什么是ORM

ORM 是 Object Relational Mapping 的缩写，译为“对象关系映射”

ORM 是一种双向数据交互技术，它不仅可以将对象中的数据存储到数据库中，也可以反过来将数据库中的数据提取到对象中

## 2、什么是Hibernate

Hibernate是一个开放源代码的对象关系映射框架

它对JDBC进行了非常轻量级的对象封装

它将POJO与数据库表建立映射关系，是一个全自动的orm框架

hibernate可以自动生成SQL语句，自动执行，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。

## 3、Hibernate如何关联对象和数据库

Hibernate需要知道怎样去加载（load）和存储（store）持久化类的对象。这正是Hibernate映射文件发挥作用的地方。

映射文件告诉Hibernate它，应该访问数据库(database)里面的哪个表（table）及应该使用表里面的哪些字段（column）。

# 二、Hibernate工程配置

## 1、开发环境

IDE：idea 2022.1

构建工具：maven3.8.6

服务器：tomcat8

jdk：1.8

Hibernate：5.3.2

Junit：4.13.2

Mysql：8.0.15

## 2、创建maven工程

#### a>创建项目，添加maven

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623174332224.png" alt="image-20220623174332224" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623174509387.png" style="zoom: 50%;" />

#### b>修改pom.xml，添加依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-core</artifactId>
        <version>5.3.2.Final</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.15</version>
    </dependency>

    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
    </dependency>

</dependencies>
```

#### c>添加模块Hibernate

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623175111688.png" alt="image-20220623175111688" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623175148923.png" alt="image-20220623175148923" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623175227783.png" alt="image-20220623175227783" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623175258299.png" alt="image-20220623175258299" style="zoom:50%;" />

## 3、配置hibernate.cfg.xml

```xml
<session-factory>
<!--    数据库连接信息-->
    <property name="connection.url">jdbc:mysql://localhost:3306/liberary?serverTimezone=GMT%2B8</property>
    <property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>
    <property name="connection.username">ningct</property>
    <property name="connection.password">ning502502502</property>
<!--    数据库方言-->
      <property name="hibernate.dialect">org.hibernate.dialect.MySQL5InnoDBDialect</property>
<!--    是否在控制台打印SQL-->
      <property name="show_sql">true</property>
<!--    对SQL进行格式化-->
      <property name="format_sql">true</property>
<!--    自动生成数据表策略-->
      <property name="hbm2ddl.auto">update</property>
  </session-factory>
```

>connection.url：
>
>​	需要添加数据库名称/liberary
>
>​	加入数据库连接时区参数serverTimezone=GMT%2B8
>
>数据库方言：
>
>​	每种数据库自己的语言，甚至每个版本都不一样，Hibernate需要知道连接数据库的方言

## 4、连接数据库

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623180337078.png" alt="image-20220623180337078" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623180357249.png" alt="image-20220623180357249" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623180545256.png" alt="image-20220623180545256" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623180616805.png" alt="image-20220623180616805" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623180737764.png" alt="image-20220623180737764" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623180809048.png" alt="image-20220623180809048" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623180848405.png" alt="image-20220623180848405" style="zoom:50%;" />

## 5、Hibernate测试

#### a>关联实体类和数据库表

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623181052720.png" alt="image-20220623181052720" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623181117715.png" alt="image-20220623181117715" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623181208251.png" alt="image-20220623181208251" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623181236980.png" alt="image-20220623181236980" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623181309617.png" alt="image-20220623181309617" style="zoom:50%;" />

> idea会自动生成映射文件，[类名].hbm.xml并在核心配置文件中进行关联
>
> 映射文件可以把实体类和数据库表对应起来，属性对应字段，值对应记录

#### b>生成实体类的测试方法

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623181444934.png" alt="image-20220623181444934" style="zoom:50%;" />

```java
/**
 * 查询数据库数据
 */
@Test
public void testQuery() {
    //Hibernate 加载核心配置文件（有数据库连接信息）
    Configuration configuration = new Configuration().configure();
    //创建一个 SessionFactory 用来获取 Session 连接对象
    SessionFactory sessionFactory = configuration.buildSessionFactory();
    //获取session 连接对象
    Session session = sessionFactory.openSession();
    //开始事务
    Transaction transaction = session.beginTransaction();
    //根据主键查询 user 表中的记录
    AccountEntity accountEntity = session.get(AccountEntity.class, 1);
    System.out.println(accountEntity);
    //提交事务
    transaction.commit();
    //释放资源
    session.close();
    sessionFactory.close();
}
```

# 三、持久化对象

## 1、什么是持久化

狭义的理解，“持久化”仅仅指把对象永久保存到数据库中



广义的理解，“持久化”包括和数据库相关的各种操作：

​	保存：把对象永久保存到数据库中。

​	更新：更新数据库中对象(记录)的状态。

​	删除：从数据库中删除一个对象。

​	查询：根据特定的查询条件，把符合查询条件的一个或多个对象从数据库加载到内存中。

​	加载：根据特定的**OID**，把一个对象从数据库加载到内存中

> 为了在系统中能够找到所需对象，需要为每一个对象分配一个唯一的标识号
>
> 在关系数据库中称之为主键，而在对象术语中，则叫做对象标识(Object identifier-OID)

## 2、持久化类

### a>什么是持久化类

持久化类（Persistent Object ）简称 PO，在 Hibernate 中， PO 是由 POJO（即 java 类或实体类）和 hbm 映射配置组成。

简单点说，持久化类本质上就是一个与数据库表建立了映射关系的普通 Java 类（实体类），例如 User 类与数据库中 user 表通过映射文件 User.hbm.xml 建立了映射关系，此时 User 就是一个持久化类。

### b>持久化类的规范

持久化类需要遵守一定的规范，具体如下：

- 持久化类中需要提供一个使用 public 修饰的无参构造器；

  > 使Hibernate可以使用Constructor.newInstance() 来实例化持久化类

- 持久化类中需要提供一个标识属性 OID，与数据表主键字段向对应，例如实体类 User 中的 id 属性。

  > 为了保证 OID 的唯一性，OID 应该由 Hibernate 进行赋值，尽量避免人工手动赋值；

- 持久化类中所有属性（包括 OID）都要与数据库表中的字段相对应，且都应该符合 JavaBean 规范，即属性使用 private 修饰，且提供相应的 setter 和 getter 方法；

- 标识属性应尽量使用基本数据类型的包装类型，例如 Interger

  > 目的是为了与数据库表的字段默认值 null 保持一致；

- 不能用 final 修饰持久化类

  > 在运行时生成代理是 Hibernate 的一个重要的功能. 如果持久化类没有实现任何接口, Hibnernate 使用 CGLIB 生成代理. 如果使用的是 final 类, 则无法生成 CGLIB 代理

- **重写** **eqauls** **和** **hashCode** 方法

  > 如果需要把持久化类的实例放到 Set 中(当需要进行关联映射时), 则应该重写这两个方法

### c>持久化对象

“持久化对象”就是持久化类的实例对象，它与数据库表中一条记录相对应，Hibernate 通过操作持久化对象即可实现对数据库表的 CRUD 操作。

> 在 Hibernate 中，持久化对象是存储在一级缓存中的，一级缓存指 Session 级别的缓存，它可以根据缓存中的持久化对象的状态改变同步更新数据库

## 3、持久化对象状态

### a>三种状态

| 状态                 | 别名           | 产生时机                                                     |                             特点                             |
| -------------------- | :------------- | ------------------------------------------------------------ | :----------------------------------------------------------: |
| 瞬时态（transient）  | 临时态或自由态 | 由 new 关键字开辟内存空间的对象（即使用 new 关键字创建的对象） | 没有唯一标识 OID；未与任何 Session 实例建立关联关系；数据库中也没有与之相关的记录； |
| 持久态（persistent） | -              | 当对象加入到 Session 的一级缓存中时，与 Session 实例建立关联关系时 | 存在唯一标识 OID，且不为 null；已经纳入到 Session 中管理；数据库中存在对应的记录；持久态对象的任何属性值的改动，都会在事务结束时同步到数据库表中。 |
| 脱管态（detached）   | 离线态或游离态 | 持久态对象与 Session 断开联系时                              | 存在唯一标识 OID；与 Session 断开关联关系，未纳入 Session 中管理；一旦有 Session 再次关联该脱管对象，那么该对象就可以立马变为持久状态；脱管态对象发生的任何改变，都不能被 Hibernate 检测到，更不能提交到数据库中。 |

### b>状态转化

![持久化对象状态转换](https://ningct.oss-cn-hangzhou.aliyuncs.com/10543955J-0.png)

通过上图可知，持久化对象的状态转换遵循以下规则：

- 当一个实体类对象通过 new 关键字创建时，此时该对象就处于瞬时态。
- 当执行 Session 接口提供的 save() 或 saveOrUpate() 方法，将瞬时态对象保存到 Session 的一级缓存中时，该对象就从瞬时态转换为了持久态。
- 当执行 Session 接口提供的 evict()、close() 或 clear() 方法，将持久态对象与 Session 断开关联关系时，该对象就从持久态转换为了脱管态。
- 当执行 Session 接口提供的 update()、saveOrUpdate() 方法，将脱管态对象重新与 Session 建立关联关系时，该对象会从脱管态转换为持久态。
- 直接执行 Session 接口提供的 get()、load() 或 find() 方法从数据库中查询出的对象，是处于持久态的。
- 当执行 Session 接口提供的 delete() 方法时，持久态对象就会从持久态转换为瞬时态。
- 由于瞬时态和脱管态对象都不在 Session 的管理范围内，因此一段时间后，它们就会被 JVM 回收。

# 四、Session

## 1、Session简介

Session 接口是 Hibernate 向应用程序提供的操纵数据库的最主要的接口, 它提供了基本的保存，更新, 删除和加载 Java对象的方法

Session具有一个缓存，位于缓存中的对象称为持久化对象，它和数据库中的相关记录对应。

Session 能够在某些时间点, 按照缓存中对象的变化来执行相关的 SQL 语句, 来同步更新数据库, 这一过程被称为刷新缓存(flush)

> 注意：Hibernate 中的 Session 与 Java Web 中的 HttpSession 没有任何关系

## 2、Session获取

### a>调用 SessionFactrory 提供的 openSession() 方法

```java
session = sessionFactory.openSession();
```



### b>用SessionFactrory 提供的 getCurrentSession() 方法

```java
session = sessionFactory.getCurrentSession();
```



> 以上 2 种方式都能获取 Session 对象，但两者的区别在于：
>
> - 采用 openSession() 方法获取 Session 对象时，SessionFactory 直接创建一个新的 Session 实例，且在使用完成后需要调用 close() 方法进行手动关闭。
> - 采用 getCurrentSession() 方法创建的 Session 实例会被绑定到当前线程中，它在事务提交或回滚后会自动关闭。

## 3、Session操作

### a>persist()和save()

使一个临时对象转变为持久化对象

完成以下操作:
	1)把 News 对象加入到 Session 缓存中, 使它进入持久化状态
	2)选用映射文件指定的标识符生成器, 为持久化对象分配唯一的 OID

>  在 使用代理主键的情况下, setId() 方法为 News 对象设置 OID 使无效的

​	3)计划执行一条 insert 语句：在 flush 缓存的时候

> persist() 和 save() 区别：
> 当对一个 OID 不为 Null 的对象执行 save() 方法时, 会把该对象以一个新的 oid 保存到数据库中;  但执行 persist() 方法时会抛出一个异常.

### b>get() 和 load() 

都可以根据跟定的 OID 从数据库中加载一个持久化对象

> 区别:
> 当数据库中不存在与 OID 对应的记录时,
>
> ​	load() 方法抛出 ObjectNotFoundException 异常,
>
> ​	get() 方法返回 null
> 两者采用不同的延迟检索策略：
>
> ​	load 方法支持延迟加载策略
>
> ​	get 不支持

### c>update() 

Session 的 update() 方法使一个游离对象转变为持久化对象, 并且计划执行一条 update 语句

若希望 Session 仅当修改了 News 对象的属性时, 才执行 update() 语句, 可以把映射文件中 <class> 元素的 select-before-update 设为 true

> 该属性的默认值为 false



当 update() 方法关联一个游离对象时, 如果在 Session 的缓存中已经存在相同 OID 的持久化对象, 会抛出异常

当 update() 方法关联一个游离对象时, 如果在数据库中不存在相应的记录, 也会抛出异常

### d>saveOrUpdate() 

Session 的 saveOrUpdate() 方法同时包含了 save() 与 update() 方法的功能

### e>merge() 

![merge](C:/Users/ning chuang tao/AppData/Roaming/Typora/typora-user-images/image-20220624180155319.png)

### f>delete() 

Session 的 delete() 方法既可以删除一个游离对象, 也可以删除一个持久化对象

Session 的 delete() 方法处理过程
	计划执行一条 delete 语句
	把对象从 Session 缓存中删除, 该对象进入删除状态

> Hibernate 的 cfg.xml 配置文件中有一个 hibernate.use_identifier_rollback 属性, 其默认值为 false, 若把它设为 true, 将改变 delete() 方法的运行行为: delete() 方法会把持久化对象或游离对象的 OID 设置为 null, 使它们变为临时对象

### e>createQuery()和createSQLQuery()

获取 Hibernate 查询对象



获取 SQL 查询对象

## 4、Session缓存

### a>简介

在 Session 接口的实现中包含一系列的 Java 集合, 这些 Java 集合构成了 Session 缓存。只要 Session 实例没有结束生命周期, 且没有清理缓存，则存放在它缓存中的对象也不会结束生命周期。

Session 缓存可减少 Hibernate 应用程序访问数据库的频率。

### b>flush

flush：Session 按照缓存中对象的属性变化来同步更新数据库

默认情况下Session在以下时间点刷新缓存：

​	显式调用Session的flush()方法

​	当应用程序调用Transaction的commit（）方法的时, 该方法先 flush ，然后在向数据库提交事务

​	当应用程序执行一些查询(HQL, Criteria)操作时，如果缓存中持久化对象的属性已经发生了变化，会先 flush 缓存，以保证查询结果能够反映持久化对象的最新状态

> flush 缓存的例外情况: 
>
> ​	如果对象使用 native 生成器生成 OID, 那么当调用 Session 的 save() 方法保存对象时, 会立即执行向数据库插入该实体的 insert 语句

commit() 和 flush() 方法的区别：

​	flush 执行一系列 sql 语句，但不提交事务；

​	commit 方法先调用flush() 方法，然后提交事务。

提交事务意味着对数据库操作永久保存下来。

### c>设定刷新缓存的时间点

通过 Session 的 setFlushMode() 方法显式设定 flush 的时间点

![image-20220624172051058](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220624172051058.png)

# 五、核心配置文件

## 1、dtd 信息

规定了 XML 中的语法和格式。

可以在 Hibernate 核心 Jar 包（hibernate-core-xxx.jar）下的 org.hibernate.hibernate-configuration-3.0.dtd 中找到

## 2、property 元素

在 <session-factory> 元素中，包含了多个 <property> 子元素，这些元素用于配置 Hibernate 连接数据库的各种信息，例如，数据库的方言、驱动、URL、用户名、密码等



这些 property 属性中，有些是 Hibernate 的必需配置，有些则是可选配置

| property 属性名                 |                             描述                             | 是否为必需属性 |
| ------------------------------- | :----------------------------------------------------------: | -------------- |
| connection.url                  |                      指定连接数据库 URL                      | 是             |
| hibernate.connection.username   |                       指定数据库用户名                       | 是             |
| hibernate.connection.password   |                        指定数据库密码                        | 是             |
| connection.driver_class         |                      指定数据库驱动程序                      | 是             |
| hibernate.dialect               | 指定数据库使用的 SQL 方言，用于确定 Hibernate 自动生成的 SQL 语句的类型 | 是             |
| hibernate.show_sql              |              用于设置是否在控制台输出 SQL 语句               | 否             |
| hibernate.format_sql            |           用于设置是否格式化控制台输出的 SQL 语句            | 否             |
| hibernate.hbm2ddl.auto          | 当创建 SessionFactory 时，是否根据映射文件自动验证表结构或自动创建、自动更新数据库表结构。 该参数的取值为 validate 、update、create 和 create-drop | 否             |
| hibernate.connection.autocommit |                       事务是否自动提交                       | 否             |

## 3、mapping元素

可以包含一个或多个 <mapping> 元素，它们用来指定 Hibernate 映射文件的信息，加载映射文件。

> 但需要注意的是，<mapping> 元素指定的是映射文件的路径，而不是包结构。

### 4、hibernate.jdbc.fetch_size和hibernate.jdbc.batch_size

hibernate.jdbc.fetch_size：实质是调用 Statement.setFetchSize() 方法设定 JDBC 的 Statement 读取数据的时候每次从数据库中取出的记录条数。


hibernate.jdbc.batch_size：设定对数据库进行批量删除，批量更新和批量插入的时候的批次大小，类似于设置缓冲区大小的意思。batchSize 越大，批量操作时向数据库发送sql的次数越少，速度就越快。


# 六、映射文件

## 1、概述

POJO 类和关系数据库之间的映射可以用一个XML文档来定义。

通过 POJO 类的数据库映射文件，Hibernate可以理解持久化类和数据表之间的对应关系，也可以理解持久化类属性与数据库表列之间的对应关系

在运行时 Hibernate 将根据这个映射文件来生成各种 SQL 语句

映射文件的扩展名为 .hbm.xml

## 2、dtd信息

可以在 Hibernate 的核心 Jar 包（hibernate-core-xxx.jar）下的 org.hibernate.hibernate-mapping-3.0.dtd 文件中找到

## 3、hibernate-mapping 元素

<hibernate-mapping> 是 Hibernate 映射文件的根元素，在该元素中定义的配置在整个映射文件中都有效。



| 属性名          | 描述                                                         | 是否必需 |
| --------------- | ------------------------------------------------------------ | -------- |
| schema          | 指定映射文件所对应的数据库名字空间                           | 否       |
| package         | 为映射文件对应的实体类指定包名                               | 否       |
| catalog         | 指定映射文件所对应的数据库目录                               | 否       |
| default-access  | 指定 Hibernate 用于访问属性时所使用的策略，默认为 property。当 default-access="property" 时，使用 getter 和 setter 方法访问成员变量；当 default-access = "field"时，使用反射访问成员变量。 | 否       |
| default-cascade | 指定默认的级联风格                                           | 否       |
| default-lazy    | 指定 Hibernate 默认使用的延迟加载策略                        | 否       |

## 4、class 元素

<class> 元素是 Hibernate 映射文件的根元素 <hibernate-mapping> 的子元素，它主要用来定义一个实体类与数据库表之间的映射关系



| 属性名  | 描述                                                         | 是否必需 |
| ------- | ------------------------------------------------------------ | -------- |
| name    | 实体类的完全限定名（包名+类名），若根元素 <hibernate-mapping> 中已经指定了 package 属性，则该属性可以省略包名 | 否       |
| table   | 对应的数据库表名。                                           | 否       |
| catalog | 指定映射文件所对应的数据库 catalog 名称，若根元素 <hibernate-mapping> 中已经指定 catalog 属性，则该属性会覆盖根元素中的配置。 | 否       |
| schema  | 指定映射文件所对应的数据库 schema 名称，若根元素 <hibernate-mapping> 中已经指定 schema 属性，则该属性会覆盖根元素中的配置。 | 否       |
| lazy    | 指定是否使用延迟加载。                                       | 否       |

## 5、id元素

通常情况下，Hibernate 推荐我们在持久化类（实体类）中定义一个标识属性，用于唯一地标识一个持久化实例（实体类对象），且标识属性需要映射到底层数据库的主键上。

在 Hibernate 映射文件中，标识属性通过 <id> 元素来指定



| 属性名 | 描述                                                         | 是否必需 |
| ------ | ------------------------------------------------------------ | -------- |
| name   | 与数据库表主键向对应的实体类的属性                           | 否       |
| column | 数据库表主键的字段名                                         | 否       |
| type   | 用于指定数据表中的字段需要转化的类型，这个类型既可以是 Hibernate 类型，也可以是 Java 类型 | 否       |

## 6、主键生成器

### a>简介

<id> 元素中通常还包含一个 <generator> 元素，该元素通过 class 属性来指定 Hibernate 的主键生成策略。

```xml
<id name="id" column="id" type="integer">
    <!--主键生成策略-->
     <generator class="native" ></generator>
</id>
```



### b>生成策略

| 主键生成策略 | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| increment    | 自动增长策略之一，适合 short、int、long 等类型的字段。该策略不是使用数据库的自动增长机制，而是使用 Hibernate 框架提供的自动增长方式，即先从表中查询主键的最大值， 然后在最大值的基础上+1。该策略存在多线程问题，一般不建议使用。 |
| identity     | 自动增长策略之一，适合 short、int、long 等类型的字段。该策略采用数据库的自动增长机制，但该策略不适用于 Oracle 数据库。 |
| sequence     | 序列，适合 short、int、long 等类型的字段。该策略应用在支持序列的数据库，例如 Oracle 数据库，但不是适用于 MySQL 数据库。 |
| uuid         | 适用于字符串类型的主键，采用随机的字符串作为主键。           |
| native       | 本地策略，Hibernate 会根据底层数据库不同，自动选择适用 identity 还是 sequence 策略，该策略也是最常用的主键生成策略。 |
| assigned     | Hibernate 框架放弃对主键的维护，主键由程序自动生成。         |
| foreign      | 主键来自于其他数据库表（应用在多表一对一的关系）。           |

## 7、property元素

<class> 元素中可以包含一个或多个 <property> 子元素，它用于表示实体类的普通属性（除与数据表主键字段对应的属性之外的其他属性）和数据表中非主键字段的映射关系

| 属性名   | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| name     | 实体类属性的名称                                             |
| column   | 数据表字段名                                                 |
| type     | 用于指定数据表中的字段需要转化的类型，这个类型既可以是 Hibernate 类型，也可以是 Java 类型 |
| length   | 数据表字段的长度                                             |
| lazy     | 该属性使用延迟加载，默认值是 false                           |
| unique   | 是否对该字段使用唯一性约束。                                 |
| not-null | 是否允许该字段为空                                           |

> 此外，在 Hibernate 映射文件中，父元素中子元素必须遵循一定的配置顺序，例如在 <class> 元素中必须先定义 <id>元素，再定义 <property> 元素，否则 Hibernate 的 XML 解析器在运行时会报错

# 七、核心接口

## 1、Configuration

### a>简介

Configuration 主要用于管理 Hibernate 配置信息，并在启动 Hibernate 应用时，创建 SessionFactory 实例

在 Hibernate 应用启动时，需要获取一些基本信息，例如，数据库 URL、数据库用户名、数据库用户密码、数据库驱动程序和数据库方言等等。这些属性都可以在 Hibernate 的核心配置文件（hiberntae.cfg.xml）中进行设置

### b>创建 Configuration 实例

Hibernate 通常使用以下方式创建 Configuration 实例，此时 Hibernate 会自动在当前项目的类路径（CLASSPATH）中，搜索核心配置文件 hibernate.cfg.xml 并将其加载到内存中，作为后续操作的基础配置 。

```java
Configuration configuration = new Configuration().configure();
```


若 Hibernate 核心配置文件没有在项目的类路径（ src 目录）下，则需要在 configure() 方法中传入一个参数指定配置文件位置，示例代码如下：

```java
Configuration configuration = new Configuration().configure("/xxxx/hibernate.cfg.xml");
```

### c>加载映射文件

我们知道，在 Hibernate 的核心配置文件中，<mapping> 元素用来指定 Hibernate 映射文件的位置信息，加载映射文件，还可以通过 2 种方式加载映射文件。

\1. 调用 Configuration 提供的 addResource() 方法，并在该方法中传入一个参数指定映射文件的位置，示例代码如下。

```java
configuration.addResource("net/biancheng/www/mapping/User.hbm.xml");
```


\2. 调用 Configuration 提供的 addClass() 方法，并将与映射文件对应的实体类以参数的形式传入该方法中，示例代码如下。

```java
configuration.addClass(User.class);
```


使用 addClass() 方法加载映射文件时，需要同时满足以下 2 个条件，否则将无法加载映射文件。

- 映射文件的命名要规范，即映射文件要完全按照“实体类.hbm.xml ”的形式命名；
- 映射文件要与实体类在同一个包下



> 需要注意的是，Configuration 对象只存在于系统的初始化阶段，将 SessionFactory 创建完成后，它的使命就完成了。

## 2、 SessionFactory

### a>简介

SessionFactory 对象用来读取和解析映射文件，并负责创建和管理 Session 对象。

SessionFactory 对象中保存了当前的数据库配置信息、所有映射关系以及 Hibernate 自动生成的预定义 SQL 语句，同时它还维护了 Hibernate 的二级缓存。

一个 SessionFactory 实例对应一个数据库存储源，Hibernate 应用可以从 SessionFactory 实例中获取 Session 实例。

SessionFactory 具有以下特点：

- SessionFactory 是线程安全的，它的同一个实例可以被应用多个不同的线程共享。
- SessionFactory 是重量级的，不能随意创建和销毁它的实例。如果应用只访问一个数据库，那么在应用初始化时就只需创建一个 SessionFactory 实例；如果应用需要同时访问多个数据库，那么则需要为每一个数据库创建一个单独的 SesssionFactory 实例。

> SessionFactory 之所以是重量级的，是因为它需要一个很大的缓存，用来存放预定义 SQL 语句以及映射关系数据等内容。我们可以为 SessionFactory 配置一个缓存插件，它被称为 Hiernate 的二级缓存

### b>创建 SessionFactory 对象

通常情况下，我们通过 Configuration 的 buildSessionFactory() 方法来创建 SessionFactory 的实例对象，示例代码如下。

```java
SessionFactory sessionFactory = configuration.buildSessionFactory();
```

### c>抽取工具类

由于 SessionFactory 是重量级的，它的实例不能随意创建和销毁，因此在实际开发时，通常会抽取出一个工具类，将 SessionFactory 对象的创建过程放在静态代码快中，以避免 SessionFactory 对象被多次创建。

## 3、Session

### a>简介

Session 是 Hibernate 应用程序与数据库进行交互时，使用最广泛的接口，它也被称为 Hibernate 的持久化管理器，所有持久化对象必须在 Session 的管理下才可以进行持久化操作。持久化类只有与 Session 关联起来后，才具有了持久化的能力。

Session 具有以下特点：

- 不是线程安全的，因此应该避免多个线程共享同一个 Session 实例；
- Session 实例是轻量级的，它的创建和销毁不需要消耗太多的资源。通常我们会将每一个Session 实例和一个数据库事务绑定，每执行一个数据库事务，不论执行成功与否，最后都因该调用 Session 的 Close() 方法，关闭 Session 释放占用的资源。


Session 对象维护了 Hibernate 的一级缓存，在显式执行 flush 之前，所有的持久化操作的数据都缓存在 Session 对象中。

> 这里的持久化指的是对数据库数据的保存、更新、删除和查询。持久化类指与数据库表建立了映射关系的实体类，持久化对象指的是持久化类的对象。

### b>创建 Session 对象

我们可以通过以下 2 个方法创建 Session 对象：

1.调用 SessionFactrory 提供的 openSession() 方法，来获取 Session 对象，示例代码如下。

```java
//采用 openSession() 方法获取 Session 对象Session session = sessionFactory.openSession();
```


\2. 调用SessionFactrory 提供的 getCurrentSession() 方法，获取 Session 对象，示例代码如下。

```java
//采用 getCurrentSession() 方法获取 Session 对象Session session = sessionFactory.getCurrentSession();
```



> 以上 2 种方式都能获取 Session 对象，但两者的区别在于：
>
> - 采用 openSession() 方法获取 Session 对象时，SessionFactory 直接创建一个新的 Session 实例，且在使用完成后需要调用 close() 方法进行手动关闭。
> - 采用 getCurrentSession() 方法创建的 Session 实例会被绑定到当前线程中，它在事务提交或回滚后会自动关闭。

## 4、Transaction

### a>简介

Transaction 是 Hibernate 提供的数据库事务管理接口，它对底层的事务接口进行了封装。

所有的持久化操作（即使是只读操作）都应该在事务管理下进行，因此在进行 CRUD 持久化操作之前，必须获得 Trasaction 对象并开启事务。

### b>获取 Transaction 对象

我们可以通过 Session 提供的以下两个方法来获取 Transaction 对象：

\1. 调用 Session 提供的 beginTransaction() 方法获取 Transaction 对象，示例代码如下。

```java
Transaction transaction = session.beginTransaction();
```


\2. 调用 Session 提供的 getTransaction() 方法获取 Transaction 对象，示例代码如下。

```java
Transaction transaction1 = session.getTransaction();
```



> 以上两个方法均可以获取 Transaction 对象，但两者存在以下不同：
>
> - getTransaction() 方法：根据 Session 获得一个 Transaction 对象，但是并没有开启事务。
> - beginTransaction() 方法：是在根据 Session 获得一个 Transaction 对象后，又继续调用 Transaction 的 begin() 方法，开启了事务。

### c>事务的隔离级别

#### 1）并发问题

脏读: 对于两个事物 T1, T2, T1 读取了已经被 T2 更新但还没有被提交的字段. 之后, 若 T2 回滚, T1读取的内容就是临时且无效的

不可重复读: 对于两个事物 T1, T2, T1 读取了一个字段, 然后 T2 更新了该字段. 之后, T1再次读取同一个字段, 值就不同了

幻读: 对于两个事物 T1, T2, T1 从一个表中读取了一个字段, 然后 T2 在该表中插入了一些新的行. 之后, 如果 T1 再次读取同一个表, 就会多出几行

#### 2）隔离级别

![image-20220624211046895](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220624211046895.png)

#### 3）设置隔离级别

JDBC 数据库连接使用数据库系统默认的隔离级别.



在 Hibernate 的配置文件中可以显式的设置隔离级别



Hibernate 通过为 Hibernate 映射文件指定 hibernate.connection.isolation 属性来设置事务的隔离级别



每一个隔离级别都对应一个整数:

1. READ UNCOMMITED
2. READ COMMITED
3. REPEATABLE READ
4. SERIALIZEABLE

### d、常用方法

| 方法       | 描述               |
| ---------- | ------------------ |
| begin()    | 该方法用于开启事务 |
| commit()   | 该方法用于提交事务 |
| rollback() | 该方法用于回滚事务 |

## 5、Query

### a>简介

Query 是 Hibernate 提供的查询接口，主要用执行 Hinernate 的查询操作。

Query 对象中通常包装了一个 HQL（Hibernate Query Language）语句，HQL 语句与 SQL 语句存在相似之处，但 HQL 语句是面向对象的，它使用的是类名和类的属性名，而不是表名和表中的字段名。HQL 能够提供更加丰富灵活、更为强大的查询能力，因此 Hibernate 官方推荐使用 HQL 进行查询。

### b>步骤

在 Hibernate 应用中使用 Query 对象进行查询，通常需要以下步骤：
		获得 Hibernate 的 Sesssin 对象；
		定义 HQL 语句；
		调用 Session 接口提供的 createQuery() 方法创建 Query 对象，并将 HQL 语句以参数的形式传入到该方法中；
		若 HQL 语句中包含参数，则调用 Query 接口提供的 setXxx() 方法设置参数；
		调用 Query 的 getResultList() 方法，进行查询，并获取查询结果集。

### c>常用方法

| 方法                               | 描述                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| setXxx()                           | Query 接口中提供了一系列 setXxx() 方法，用于设置查询语句中的参数。这些方法都需要两个参数，分别是：参数名或占位符位置、参数值。我们需要根据参数类型的不同，分别调用不同的 setXxx() 方法，例如 setString()、setInteger()、setLong()、setBoolean() 和 setDate() 等等。 |
| Iterator<R> iterate();             | 该方法用于执行查询语句，并返回一个 Iterator 对象。我们可以通过返回的 Iterator 对象，遍历得到结果集。 |
| Object uniqueResult();             | 该方法用户执行查询，并返回一个唯一的结果。使用该方法时，需要确保查询结果只有一条数据，否则就会报错。 |
| int executeUpdate();               | 该方法用于执行 HQL 的更新和删除操作。                        |
| Query<R> setFirstResult(int var1); | 该方法用户设置结果集第一条记录的位置，即设置从第几条记录开始查询，默认从 0 开始。 |
| Query<R> setMaxResults(int var1);  | 该方法用于设置结果集的最大记录数，通常与 setFirstResult() 方法结合使用，用于限制结果集的范围，以实现分页功能。 |

# 八、关系映射

## 1、继承映射

### a>每个继承层次一张表

这种映射方式整个继承层次结构中所有类的属性映射为一张表中的字段。

增加鉴别字段：为了区分来自不同的类的映射，需要在表中增加一个用于鉴别不同类实体的字段。

#### 1)映射语法：

分别为继承层次的各个类定义持久化类，其中只有父类包含标识符属性
定义一个以父类命名的映射文件
在映射文件中的后面添加元素，并使用该元素的column属性定义鉴别字段
在映射文件中，父类的属性直接放在标记对之间，子类的属性放在标记对之间
在类/子类标记中使用discriminator-value属性指定各个类对应的鉴别字段的值

#### 2)优缺点

优点：

​		最简单、执行效率最高（因为无需进行任何关联操作）

缺点：

​		存在冗余字段

​		在数据表中需要加入额外的区分各个类的字段

​		不允许为子类的属性对应的字段定义为not null约束

### b>每个具体类一张表

这种映射方式为每个子类创建一张表，每张表包含子类的所有属性，包括继承的父类的属性

#### 1)映射语法

分别为继承层次的各个类定义持久化类，其中只有父类包含标识符属性
定义父类为抽象类，每个子类为具体类
定义一个以父类命名的映射文件
父类不映射成表，每个子类映射一张表
在映射文件中，父类属性放在标记对之间设置，子类属性放在标记对之间设置

#### 2）优缺点

优点：

​		数据结构清晰

​		可以对子类的属性映射的字段定义not null约束

缺点：

​		子表的主键不能重复

​		不能使用数据库的自增方式生成主键

​		父类属性重复出现在多张表中

### c>每个类一张表

为每个类创建一张表，每张表仅包含当前类的属性，不包含父类的属性

#### 1)映射语法：

分别为继承层次的各个类定义持久化类，其中只有父类包含标识符属性
定义一个以父类命名的映射文件
父类和子类分别映射到不同的表，父类与子类对应的表通过外键关联
在映射文件中，父类属性放在标记对之间，子类属性放在映射文件中的标记对之间

#### 2)优缺点

优点：

​		数据结构层次清晰，没有冗余

​		且可以对子类的属性映射的字段定义not null约束
缺点：

​		类的继承层次比较多时，需要关联的表也越多，查询性能不如每个类继承结构一张表

## 2、集合映射

### a>简介

在持久化类中，有时会使用到值类型的对象属性。

所谓值类型的对象，是指它对应的类没哟偶对象标示符属性，只能作为一个持久化类的属性使用。

如果持久化类中有一个值类型的集合，那么就需要一张额外的数据库表来保存这个值类型集合的数据，这张表被称为集合表。

### b>映射set

```xml
<!--Name属性：指定要映射的属性名
	Table属性：指定对应的数据库表名-->
<set name="hobbies" table="student_hobby" >
    <!--key 子元素：指定集合属性对应的表的外键列-->
    <key column="student_id"/>
 	<!--element 子元素：映射集合内的元素-->
    <element type="string" column="hobby_name" not-null="true"/>
</set> 
```

### c>映射list

```xml
<!--List元素用来映射java.util.List类型的属性
	Name属性：指定要映射的属性名
	Table属性：指定对应的数据库表名--> 
<list name=”hobbies” table=”student_hobby”>
       <!--key 元素：指定集合属性对应的表的外键列-->
       <key column=”student_id”/>
       <!--list-index 元素：指定索引列-->
       <list-index column=”position”/>
       <!--element 元素：映射集合内的元素-->
       <element type=”string” column=”hobby_name” not-null=”ture”>
</list>
```

### d>映射bag

```xml
<bag name=”hobbies” table=”student_hobby”>
       <!--key 元素：指定集合属性对应的表的外键列-->
       <key column=”student_id”/>
       <!--element 元素：映射集合内的元素-->
       <element type=”string” column=”hobby_name” not-null=”true”/>
</bag>
```

### e>映射map

```xml
<!--Bag元素用来映射java.util.Collection或java.util.List类型的属性
    Name属性：指定属性的属性名
    Table属性：指定对应的数据库表名--> 
<map name=”hobbies” table=”student_hobby”>
       <!--key 元素：指定集合属性对应的表的外键列-->
       <key column=”student_id”/>
       <!--map-key 元素：指定map属性的键在对应表中的列 -->
       <map-key column=”hobby_id” type=”long”/>
       <!--element元素：映射集合内的元素-->
       <element type=”string” column=”hobby_name” not-null=”true”/>
</map>
```

### f>排序集合

在<set>元素中通过指定sort=”natural”，使用自然排序来对集合内的元素进行排序存放

### g>有序集合

有序集合是指在数据廤获取数据时就使用SQL语句的order by语句排好序，然后再封装到结合中存放。

这种情况下持久化类中的集合属性仍旧使用Set或Map

但对应的映射文件中的<set>或<map>元素需要添加order-by属性来指定SQL中order by子句

## 3、关联映射

### a>一对一关联

![image-20220625190516822](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625190516822.png)

#### 1）外键映射

对于基于外键的1-1关联，其外键可以存放在任意一边，在需要存放外键一端，增加many-to-one元素。为many-to-one元素增加unique=“true” 属性来表示为1-1关联

![image-20220625190801441](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625190801441.png)

另一端需要使用one-to-one元素，该元素使用 property-ref 属性指定使用被关联实体主键以外的字段作为关联字段

![image-20220625190811165](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625190811165.png)

不使用 property-ref 属性的 sql

![image-20220625190821630](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625190821630.png)

使用 property-ref 属性的 sql

![image-20220625190830540](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625190830540.png)

#### 2）主键映射

基于主键的映射策略:指一端的主键生成器使用 foreign 策略,表明根据”对方”的主键来生成自己的主键，自己并不能独立生成主键. <param> 子元素指定使用当前持久化类的哪个属性作为 “对方”

![image-20220625190953951](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625190953951.png)

采用foreign主键生成器策略的一端增加 one-to-one 元素映射关联属性，其one-to-one属性还应增加 constrained=“true” 属性；另一端增加one-to-one元素映射关联属性。
constrained(约束):指定为当前持久化类对应的数据库表的主键添加一个外键约束，引用被关联的对象(“对方”)所对应的数据库表主键

![image-20220625191003874](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625191003874.png)

### b>多对一单向

单向 n-1 关联只需从 n 的一端可以访问 1 的一端
域模型: 从 Order 到 Customer 的多对一单向关联需要在Order 类中定义一个 Customer 属性, 而在 Customer 类中无需定义存放 Order 对象的集合属性

![image-20220625191618857](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625191618857.png)

关系数据模型:ORDERS 表中的 CUSTOMER_ID 参照 CUSTOMER 表的主键

![image-20220625191744768](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625191744768.png)

Hibernate 使用 <many-to-one> 元素来映射多对一关联关系

![image-20220625191826537](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625191826537.png)

### c>一对多双向

双向 1-n 与 双向 n-1 是完全相同的两种情形
双向 1-n 需要在 1 的一端可以访问 n 的一端, 反之依然.
域模型:从 Order 到 Customer 的多对一双向关联需要在Order 类中定义一个 Customer 属性, 而在 Customer 类中需定义存放 Order 对象的集合属性

![image-20220625191958430](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625191958430.png)

关系数据模型:ORDERS 表中的 CUSTOMER_ID 参照 CUSTOMER 表的主键

![image-20220625192053199](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625192053199.png)

### d>多对多单向

![image-20220625192422272](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625192422272.png)

n-n 的关联必须使用连接表

与 1-n 映射类似，必须为 set 集合元素添加 key 子元素，指定 CATEGORIES_ITEMS 表中参照 CATEGORIES 表的外键为 CATEGORIY_ID.



与 1-n 关联映射不同的是，建立 n-n 关联时, 集合中的元素使用 many-to-many



 many-to-many 子元素的 class 属性指定 items 集合中存放的是 Item 对象, column 属性指定  CATEGORIES_ITEMS 表中参照 ITEMS 表的外键为 ITEM_ID

![image-20220625192604331](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625192604331.png)

### e>多对多双向

![image-20220625192627443](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625192627443.png)

双向 n-n 关联需要两端都使用集合属性



双向n-n关联必须使用连接表



集合属性应增加 key 子元素用以映射外键列, 集合元素里还应增加many-to-many子元素关联实体类



在双向 n-n 关联的两边都需指定连接表的表名及外键列的列名



 两个集合元素 set 的 table 元素的值必须指定，而且必须相同



set元素的两个子元素：key 和 many-to-many 都必须指定 column 属性，其中，key 和 many-to-many 分别指定本持久化类和关联类在连接表中的外键列名，因此两边的 key 与 many-to-many 的column属性交叉相同。也就是说，一边的set元素的key的 cloumn值为a,many-to-many 的 column 为b；则另一边的 set 元素的 key 的 column 值 b,many-to-many的 column 值为 a


对于双向 n-n 关联, 必须把其中一端的 inverse 设置为 true, 否则两端都维护关联关系可能会造成主键冲突.

## 4、级联

当拥有关联关系控制权的一方，对数据库进行操作时，为了使数据保持同步，其关联对象也执行同样的操作。

在 Hibernate 的映射文件中有一个 cascade 属性，该属性用于设置关联对象是否进行级联操作，其常用属性值，如下表。

| 属性              | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| save-update       | 在进行保存或更新时，进行级联操作。                           |
| delete            | 在进行删除操作时，进行级联操作。                             |
| delete-orphan     | 孤儿删除，删除和当前对象解除关联关系的对象。 该属性值仅在关联关系为一对多时有效，因此只有此时才会存在父子关系，其中“一”的一方为“父方”，“多”的一方为“子方”； 例如，班级和学生是一对多的关系，其中班级为“父方”，学生为“子方”，当学生与班级解除了关联关系后，其外键被重置为了 null，那么这个学生也就失去了他的学生身份，这种记录就需要从学生表中删除。 |
| all               | 所有情况下均进行级联操作， delete-orphan （孤儿删除）除外。  |
| all-delete-orphan | 所有情况下均进行级联操作， 包括 delete-orphan （孤儿删除）。 |
| none              | 默认值，表示所有情况下，均不进行级联操作。                   |

> 注意：使用 cascade 属性进行级联操作时，其必须具备管理或控制关联关系的能力，即其 inverse 属性必须取值为 false（默认值）。

## 5、组件

 在Hibernate中,component是某个实体的逻辑组成部分，它与实体的根本区别是没有oid（对象标识符），component是一个被包含的对象,它作为值类型被持久化，而非一个实体。

```xml
<component name="address" class="bean.Address">
		<property name="addressName" column="address_name"></property>
		<property name="addressValue" column="address_value"></property>
</component>
```



# 九、检索策略

## 1、概述

hibernate的Session在加载Java对象时，一般都会把鱼这个对象相关联的其他Java对象也都加载到缓存中，以方便程序的调用。

但很多情况下，我们不需要加载太多无用的对象到缓存中，一来会占用大量的内存，二来会增加数据库的访问次数，使得程序的运行效率降低。

为了合理的使用缓存，Hibernate提供了不同的检索策略来解决这些问题。

## 2、立即检索

 采用立即检索策略，会把被检索的对象，以及和这个对象关联的一对多对象都加载到缓存中。

Session的get方法就使用的立即检索策略。

这种策略的优点在于，频繁使用的对象会被加载到缓存中，程序调用很方便，也很及时。

缺点就是，占用的内存过多，而且数据库访问的次数也会很频繁，效率低下。

## 3、延迟检索

 采用延迟检索策略，就不会加载关联对象的内容。

直到第一次调用关联对象时，才去加载关联对象。

在不涉及关联类操作时，延迟检索策略只适用于Session的load方法。

涉及关联类操作时，延迟检索策略也能够适用于get，list等操作。

> 在类级别操作时， 延迟检索策略，只加载类的OID不加载类的其他属性，只用当第一次访问其他属性时，才回访问数据库去加载内容。（这里使用了CGLIB生成了类的代理类）

> 在关联级别操作时，延迟检索策略，只加载类本身，不加载关联类，直到第一次调用关联对象时，才去加载关联对象。

  这种策略的优点在于，由程序决定加载哪些类和内容，而不必全部都加载，避免了内存的大量占用和数据库的频繁访问。

缺点就是在Session关闭后，就不能访问关联类对象了。 需要确保Session一直处于打开状态，调用关联对象，最后在关闭Session对象。

## 4、迫切左外连接检索

 采用左外连接检索，能够使用Sql的外连接查询，将需要加载的关联对象加载在缓存中。<set>fetch设置为join，<many-to-one>的fetch设置为join。

 这种策略的优点在于，对应用程序完全透明，不管对象处于持久化状态，还是游离状态，应用程序都可以方便的从一个对象导航到与它关联的对象。

缺点就是可能会加载应用程序不需要访问的对象，白白浪费许多内存空间。复杂的数据库表连接也会影响检索性能。

## 5、检索场景

### a>类级别

类级别可选的检索策略包括立即检索和延迟检索， 默认为延迟检索。

立即检索: 立即加载检索方法指定的对象；
延迟检索: 延迟加载检索方法指定的对象。在使用具体的属性时，再进行加载。
类级别的检索策略可以通过 元素的 lazy 属性进行设置。

如果程序加载一个对象的目的是为了访问它的属性，可以采取立即检索；

如果程序加载一个持久化对象的目的是仅仅为了获得它的引用，可以采用延迟检索

> 需要注意懒加载异常（LazyInitializationException：简单理解该异常就是Hibernate在使用延迟加载时，并没有将数据实际查询出来，而只是得到了一个代理对象，当使用属性的时候才会去查询，而如果这个时候session关闭了，则会报该异常）

无论元素的lazy 属性是true 还是false，Session 的get() 方法及Query 的list() 方法在类级别总是使用立即检索策略。

若 元素的 lazy 属性为 true 或取默认值, Session 的 load() 方法不会执行查询数据表的 SELECT 语句，仅返回代理类对象的实例，该代理类实例有如下特征：
		由 Hibernate 在运行时采用 CGLIB 工具动态生成；
		Hibernate 创建代理类实例时，仅初始化其 OID 属性；
		在应用程序第一次访问代理类实例的非 OID 属性时, Hibernate 会初始化代理类实例。

### b>关联级别

在映射文件中, 用 <set> 元素来配置一对多关联及多对多关联关系.

 <set> 元素有 lazy 和 fetch 属性

> lazy: 主要决定 orders 集合被初始化的时机. 即到底是在加载 Customer 对象时就被初始化, 还是在程序访问 orders 集合时被初始化
> fetch: 取值为 “select” 或 “subselect” 时, 决定初始化 orders 的查询语句的形式;  若取值为”join”, 则决定 orders 集合被初始化的时机
> 若把 fetch 设置为 “join”, lazy 属性将被忽略

![image-20220625200705789](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625200705789.png)

![image-20220625200807863](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220625200807863.png)

# 十、检索方式

## 1、HQL

### a>简介

HQL(Hibernate Query Language) 是面向对象的查询语言, 它和 SQL 查询语言有些相似. 在 Hibernate 提供的各种检索方式中, HQL 是使用最广的一种检索方式

### b>大小写敏感性问题

除了Java类与属性的名称外，查询语句对大小写并不敏感

### c> from子句

Hibernate中最简单的查询语句的形式如下：

```sql
from eg.Cat
```

该子句简单的返回`eg.Cat`类的所有实例。 通常我们不需要使用类的全限定名, 因为 `auto-import`（自动引入） 是缺省的情况。 所以我们几乎只使用如下的简单写法：

```sql
from Cat
```

大多数情况下, 你需要指定一个*别名*, 原因是你可能需要 在查询语句的其它部分引用到`Cat`

```sql
from Cat as cat
```

这个语句把别名`cat`指定给类`Cat` 的实例, 这样我们就可以在随后的查询中使用此别名了。 关键字`as` 是可选的，我们也可以这样写:

```sql
from Cat cat
```

子句中可以同时出现多个类, 其查询结果是产生一个笛卡儿积或产生跨表的连接。

```sql
from Formula, Parameter
from Formula as form, Parameter as param
```

### d>关联(Association)与连接(Join)

```sql
from Cat as cat 
    inner join cat.mate as mate
    left outer join cat.kittens as kitten
```

通过HQL的`with`关键字，提供额外的join条件。

```sql
from Cat as cat 
    left join cat.kittens as kitten 
        with kitten.bodyWeight > 10.0
```

还有，一个"fetch"连接允许仅仅使用一个选择语句就将相关联的对象或一组值的集合随着他们的父对象的初始化而被初始化，这种方法在使用到集合的情况下尤其有用，对于关联和集合来说，它有效的代替了映射文件中的外联接 与延迟声明（lazy declarations）. 查看 [第 19.1 节 “ 抓取策略(Fetching strategies) ”](https://hibernate.net.cn/docs/performance.html#performance-fetching) 以获得等多的信息。

```
from Cat as cat 
    inner join fetch cat.mate
    left join fetch cat.kittens
```

一个fetch连接通常不需要被指定别名, 因为相关联的对象不应当被用在 `where` 子句 (或其它任何子句)中。同时，相关联的对象 并不在查询的结果中直接返回，但可以通过他们的父对象来访问到他们。

```
from Cat as cat 
    inner join fetch cat.mate
    left join fetch cat.kittens child
    left join fetch child.kittens
```

假若使用`iterate()`来调用查询，请注意`fetch`构造是不能使用的(`scroll()` 可以使用)。`fetch`也不应该与`setMaxResults()` 或`setFirstResult()`共用，这是因为这些操作是基于结果集的，而在预先抓取集合类时可能包含重复的数据，也就是说无法预先知道精确的行数。`fetch`还不能与独立的 `with`条件一起使用。通过在一次查询中fetch多个集合，可以制造出笛卡尔积，因此请多加注意。对bag映射来说，同时join fetch多个集合角色可能在某些情况下给出并非预期的结果，也请小心。最后注意，使用`full join fetch` 与 `right join fetch`是没有意义的。

### e> join 语法的形式

HQL支持两种关联join的形式：`implicit(隐式)` 与`explicit（显式）`。

上一节中给出的查询都是使用`explicit(显式)`形式的，其中form子句中明确给出了join关键字。这是建议使用的方式。

`implicit（隐式）`形式不使用join关键字。关联使用"点号"来进行“引用”。`implicit` join可以在任何HQL子句中出现.`implicit` join在最终的SQL语句中以inner join的方式出现。

```sql
from Cat as cat where cat.mate.name like '%s%'
```

### f>select子句

`select` 子句选择将哪些对象与属性返 回到查询结果集中. 考虑如下情况:

```
select mate 
from Cat as cat 
    inner join cat.mate as mate
```

该语句将选择`mate`s of other `Cat`s。（其他猫的配偶） 实际上, 你可以更简洁的用以下的查询语句表达相同的含义:

```sql
select cat.mate from Cat cat
```

查询语句可以返回值为任何类型的属性，包括返回类型为某种组件(Component)的属性:

```sql
select cat.name from DomesticCat cat
where cat.name like 'fri%'
select cust.name.firstName from Customer as cust
```

查询语句可以返回多个对象和（或）属性，存放在 `Object[]`队列中,

```sql
select mother, offspr, mate.name 
from DomesticCat as mother
    inner join mother.mate as mate
    left outer join mother.kittens as offspr
```

或存放在一个`List`对象中,

```sql
select new list(mother, offspr, mate.name)
from DomesticCat as mother
    inner join mother.mate as mate
    left outer join mother.kittens as offspr
```

也可能直接返回一个实际的类型安全的Java对象,

```sql
select new Family(mother, mate, offspr)
from DomesticCat as mother
    join mother.mate as mate
    left join mother.kittens as offspr
```

假设类`Family`有一个合适的构造函数.

你可以使用关键字`as`给“被选择了的表达式”指派别名:

```sql
select max(bodyWeight) as max, min(bodyWeight) as min, count(*) as n
from Cat cat
```

这种做法在与子句`select new map`一起使用时最有用:

```sql
select new map( max(bodyWeight) as max, min(bodyWeight) as min, count(*) as n )
from Cat cat
```

该查询返回了一个`Map`的对象，内容是别名与被选择的值组成的名-值映射。

### g>聚集函数

HQL查询甚至可以返回作用于属性之上的聚集函数的计算结果:

```sql
select avg(cat.weight), sum(cat.weight), max(cat.weight), count(cat)
from Cat cat
```

受支持的聚集函数如下：

- `avg(...), sum(...), min(...), max(...)`
- `count(*)`
- `count(...), count(distinct ...), count(all...)`

你可以在选择子句中使用数学操作符、连接以及经过验证的SQL函数：

```sql
select cat.weight + sum(kitten.weight) 
from Cat cat 
    join cat.kittens kitten
group by cat.id, cat.weight
select firstName||' '||initial||' '||upper(lastName) from Person
```

关键字`distinct`与`all` 也可以使用，它们具有与SQL相同的语义.

```sql
select distinct cat.name from Cat cat

select count(distinct cat.name), count(cat) from Cat cat
```

### h>多态查询

一个如下的查询语句:

```sql
from Cat as cat
```

不仅返回`Cat`类的实例, 也同时返回子类 `DomesticCat`的实例. Hibernate 可以在`from`子句中指定*任何* Java 类或接口. 查询会返回继承了该类的所有持久化子类 的实例或返回声明了该接口的所有持久化类的实例。下面的查询语句返回所有的被持久化的对象：

```sql
from java.lang.Object o
```

接口`Named` 可能被各种各样的持久化类声明：

```sql
from Named n, Named m where n.name = m.name
```

注意，最后的两个查询将需要超过一个的SQL `SELECT`.这表明`order by`子句 没有对整个结果集进行正确的排序. (这也说明你不能对这样的查询使用`Query.scroll()`方法.)

### i>where子句

`where`子句允许你将返回的实例列表的范围缩小. 如果没有指定别名，你可以使用属性名来直接引用属性:

```sql
from Cat where name='Fritz'
```

如果指派了别名，需要使用完整的属性名:

```sql
from Cat as cat where cat.name='Fritz'
```

返回名为（属性name等于）'Fritz'的`Cat`类的实例。

```sql
select foo 
from Foo foo, Bar bar
where foo.startDate = bar.date
```

将返回所有满足下面条件的`Foo`类的实例： 存在如下的`bar`的一个实例，其`date`属性等于 `Foo`的`startDate`属性。 复合路径表达式使得`where`子句非常的强大，考虑如下情况：

```sql
from Cat cat where cat.mate.name is not null
```

该查询将被翻译成为一个含有表连接（内连接）的SQL查询。如果你打算写像这样的查询语句

```sql
from Foo foo  
where foo.bar.baz.customer.address.city is not null
```

在SQL中，你为达此目的将需要进行一个四表连接的查询。

`=`运算符不仅可以被用来比较属性的值，也可以用来比较实例：

```sql
from Cat cat, Cat rival where cat.mate = rival.mate
select cat, mate 
from Cat cat, Cat mate
where cat.mate = mate
```

特殊属性（小写）`id`可以用来表示一个对象的唯一的标识符。（你也可以使用该对象的属性名。）

```sql
from Cat as cat where cat.id = 123

from Cat as cat where cat.mate.id = 69
```

第二个查询是有效的。此时不需要进行表连接！

同样也可以使用复合标识符。比如`Person`类有一个复合标识符，它由`country`属性 与`medicareNumber`属性组成。

```sql
from bank.Person person
where person.id.country = 'AU' 
    and person.id.medicareNumber = 123456
from bank.Account account
where account.owner.id.country = 'AU' 
    and account.owner.id.medicareNumber = 123456
```

第二个查询也不需要进行表连接。

同样的，特殊属性`class`在进行多态持久化的情况下被用来存取一个实例的鉴别值（discriminator value）。 一个嵌入到where子句中的Java类的名字将被转换为该类的鉴别值。

```sql
from Cat cat where cat.class = DomesticCat
```

你也可以声明一个属性的类型是组件或者复合用户类型（以及由组件构成的组件等等）。永远不要尝试使用以组件类型来结尾的路径表达式（path-expression） （与此相反，你应当使用组件的一个属性来结尾）。 举例来说，如果`store.owner`含有一个包含了组件的实体`address`

```sql
store.owner.address.city    // 正确
store.owner.address         // 错误!
```

一个“任意”类型有两个特殊的属性`id`和`class`, 来允许我们按照下面的方式表达一个连接（`AuditLog.item` 是一个属性，该属性被映射为`<any>`）。

```sql
from AuditLog log, Payment payment 
where log.item.class = 'Payment' and log.item.id = payment.id
```

注意，在上面的查询与句中，`log.item.class` 和 `payment.class` 将涉及到完全不同的数据库中的列。

### j>表达式

在`where`子句中允许使用的表达式包括 大多数你可以在SQL使用的表达式种类:

- 数学运算符`+, -, *, /`
- 二进制比较运算符`=, >=, <=, <>, !=, like`
- 逻辑运算符`and, or, not`
- `in`, `not in`, `between`, `is null`, `is not null`, `is empty`, `is not empty`, `member of` and `not member of`
- "简单的" case, `case ... when ... then ... else ... end`,和 "搜索" case, `case when ... then ... else ... end`
- 字符串连接符`...||...` or `concat(...,...)`
- `current_date()`, `current_time()`, `current_timestamp()`
- `second(...)`, `minute(...)`, `hour(...)`, `day(...)`, `month(...)`, `year(...)`,
- EJB-QL 3.0定义的任何函数或操作：`substring(), trim(), lower(), upper(), length(), locate(), abs(), sqrt(), bit_length()， mod()`
- `coalesce()` 和 `nullif()`
- `str()` 把数字或者时间值转换为可读的字符串
- `cast(... as ...)`, 其第二个参数是某Hibernate类型的名字，以及`extract(... from ...)`，只要ANSI `cast()` 和 `extract()` 被底层数据库支持
- HQL `index()` 函数，作用于join的有序集合的别名。
- HQL函数，把集合作为参数:`size(), minelement(), maxelement(), minindex(), maxindex()`,还有特别的`elements()` 和`indices`函数，可以与数量词加以限定：`some, all, exists, any, in`。
- 任何数据库支持的SQL标量函数，比如`sign()`, `trunc()`, `rtrim()`, `sin()`
- JDBC风格的参数传入 `?`
- 命名参数`:name`, `:start_date`, `:x1`
- SQL 直接常量 `'foo'`, `69`, `6.66E+2`, `'1970-01-01 10:00:01.0'`
- Java `public static final` 类型的常量 `eg.Color.TABBY`

关键字`in`与`between`可按如下方法使用:

```sql
from DomesticCat cat where cat.name between 'A' and 'B'
from DomesticCat cat where cat.name in ( 'Foo', 'Bar', 'Baz' )
```

而且否定的格式也可以如下书写：

```sql
from DomesticCat cat where cat.name not between 'A' and 'B'
from DomesticCat cat where cat.name not in ( 'Foo', 'Bar', 'Baz' )
```

同样, 子句`is null`与`is not null`可以被用来测试空值(null).

在Hibernate配置文件中声明HQL“查询替代（query substitutions）”之后， 布尔表达式（Booleans）可以在其他表达式中轻松的使用:

```sql
<property name="hibernate.query.substitutions">true 1, false 0</property>
```

系统将该HQL转换为SQL语句时，该设置表明将用字符 `1` 和 `0` 来 取代关键字`true` 和 `false`:

```sql
from Cat cat where cat.alive = true
```

你可以用特殊属性`size`, 或是特殊函数`size()`测试一个集合的大小。

```sql
from Cat cat where cat.kittens.size > 0
from Cat cat where size(cat.kittens) > 0
```

对于索引了（有序）的集合，你可以使用`minindex` 与 `maxindex`函数来引用到最小与最大的索引序数。 同理，你可以使用`minelement` 与 `maxelement`函数来 引用到一个基本数据类型的集合中最小与最大的元素。

```sql
from Calendar cal where maxelement(cal.holidays) > current_date
from Order order where maxindex(order.items) > 100
from Order order where minelement(order.items) > 10000
```

在传递一个集合的索引集或者是元素集(`elements`与`indices` 函数) 或者传递一个子查询的结果的时候，可以使用SQL函数`any, some, all, exists, in`

```sql
select mother from Cat as mother, Cat as kit
where kit in elements(foo.kittens)
select p from NameList list, Person p
where p.name = some elements(list.names)
from Cat cat where exists elements(cat.kittens)
from Player p where 3 > all elements(p.scores)
from Show show where 'fizard' in indices(show.acts)
```

注意，在Hibernate3种，这些结构变量- `size`, `elements`, `indices`, `minindex`, `maxindex`, `minelement`, `maxelement` - 只能在where子句中使用。

一个被索引过的（有序的）集合的元素(arrays, lists, maps)可以在其他索引中被引用（只能在where子句中）：

```sql
from Order order where order.items[0].id = 1234
select person from Person person, Calendar calendar
where calendar.holidays['national day'] = person.birthDay
    and person.nationality.calendar = calendar
select item from Item item, Order order
where order.items[ order.deliveredItemIndices[0] ] = item and order.id = 11
select item from Item item, Order order
where order.items[ maxindex(order.items) ] = item and order.id = 11
```

在`[]`中的表达式甚至可以是一个算数表达式。

```sql
select item from Item item, Order order
where order.items[ size(order.items) - 1 ] = item
```

对于一个一对多的关联（one-to-many association）或是值的集合中的元素， HQL也提供内建的`index()`函数，

```sql
select item, index(item) from Order order 
    join order.items item
where index(item) < 5
```

如果底层数据库支持标量的SQL函数，它们也可以被使用

```sql
from DomesticCat cat where upper(cat.name) like 'FRI%'
```

如果你还不能对所有的这些深信不疑，想想下面的查询。如果使用SQL，语句长度会增长多少，可读性会下降多少：

```sql
select cust
from Product prod,
    Store store
    inner join store.customers cust
where prod.name = 'widget'
    and store.location.name in ( 'Melbourne', 'Sydney' )
    and prod = all elements(cust.currentOrder.lineItems)
```

*提示:* 会像如下的语句

```sql
SELECT cust.name, cust.address, cust.phone, cust.id, cust.current_order
FROM customers cust,
    stores store,
    locations loc,
    store_customers sc,
    product prod
WHERE prod.name = 'widget'
    AND store.loc_id = loc.id
    AND loc.name IN ( 'Melbourne', 'Sydney' )
    AND sc.store_id = store.id
    AND sc.cust_id = cust.id
    AND prod.id = ALL(
        SELECT item.prod_id
        FROM line_items item, orders o
        WHERE item.order_id = o.id
            AND cust.current_order = o.id
    )
```

### k>order by子句

查询返回的列表(list)可以按照一个返回的类或组件（components)中的任何属性（property）进行排序：

```sql
from DomesticCat cat
order by cat.name asc, cat.weight desc, cat.birthdate
```

可选的`asc`或`desc`关键字指明了按照升序或降序进行排序.

### l>group by子句

一个返回聚集值(aggregate values)的查询可以按照一个返回的类或组件（components)中的任何属性（property）进行分组：

```
select cat.color, sum(cat.weight), count(cat) 
from Cat cat
group by cat.color
select foo.id, avg(name), max(name) 
from Foo foo join foo.names name
group by foo.id
```

`having`子句在这里也允许使用.

```
select cat.color, sum(cat.weight), count(cat) 
from Cat cat
group by cat.color 
having cat.color in (eg.Color.TABBY, eg.Color.BLACK)
```

如果底层的数据库支持的话(例如不能在MySQL中使用)，SQL的一般函数与聚集函数也可以出现 在`having`与`order by` 子句中。

```sql
select cat
from Cat cat
    join cat.kittens kitten
group by cat.id, cat.name, cat.other, cat.properties
having avg(kitten.weight) > 100
order by count(kitten) asc, sum(kitten.weight) desc
```

注意`group by`子句与 `order by`子句中都不能包含算术表达式（arithmetic expressions）. 也要注意Hibernate目前不会扩展group的实体,因此你不能写`group by cat`,除非`cat`的所有属性都不是聚集的(non-aggregated)。你必须明确的列出所有的非聚集属性。

### m>子查询

对于支持子查询的数据库，Hibernate支持在查询中使用子查询。一个子查询必须被圆括号包围起来（经常是SQL聚集函数的圆括号）。 甚至相互关联的子查询（引用到外部查询中的别名的子查询）也是允许的。

```
from Cat as fatcat 
where fatcat.weight > ( 
    select avg(cat.weight) from DomesticCat cat 
)
from DomesticCat as cat 
where cat.name = some ( 
    select name.nickName from Name as name 
)
from Cat as cat 
where not exists ( 
    from Cat as mate where mate.mate = cat 
)
from DomesticCat as cat 
where cat.name not in ( 
    select name.nickName from Name as name 
)
select cat.id, (select max(kit.weight) from cat.kitten kit) 
from Cat as cat
```

注意，HQL自查询只可以在select或者where子句中出现。

在select列表中包含一个表达式以上的子查询，你可以使用一个元组构造符（tuple constructors）：

```sql
from Cat as cat 
where not ( cat.name, cat.color ) in ( 
    select cat.name, cat.color from DomesticCat cat 
)
```

注意在某些数据库中（不包括Oracle与HSQL），你也可以在其他语境中使用元组构造符， 比如查询用户类型的组件与组合：

```sql
from Person where name = ('Gavin', 'A', 'King')
```

该查询等价于更复杂的：

```sql
from Person where name.first = 'Gavin' and name.initial = 'A' and name.last = 'King')
```

有两个很好的理由使你不应当作这样的事情：首先，它不完全适用于各个数据库平台；其次，查询现在依赖于映射文件中属性的顺序。

## 2、QBC

### a>简介

QBC：Query By Criteria，条件查询。

是一种更加[面向对象](https://so.csdn.net/so/search?q=面向对象&spm=1001.2101.3001.7020)化的查询的方式。

### b>创建一个Criteria 实例

`org.hibernate.Criteria`接口表示特定持久类的一个查询。`Session`是 `Criteria`实例的工厂。

```sql
Criteria crit = sess.createCriteria(Cat.class);
crit.setMaxResults(50);
List cats = crit.list();
```

### c>限制结果集内容

一个单独的查询条件是`org.hibernate.criterion.Criterion` 接口的一个实例。`org.hibernate.criterion.Restrictions`类 定义了获得某些内置`Criterion`类型的工厂方法。

```sql
List cats = sess.createCriteria(Cat.class)
    .add( Restrictions.like("name", "Fritz%") )
    .add( Restrictions.between("weight", minWeight, maxWeight) )
    .list();
```

约束可以按逻辑分组。

```sql
List cats = sess.createCriteria(Cat.class)
    .add( Restrictions.like("name", "Fritz%") )
    .add( Restrictions.or(
        Restrictions.eq( "age", new Integer(0) ),
        Restrictions.isNull("age")
    ) )
    .list();
List cats = sess.createCriteria(Cat.class)
    .add( Restrictions.in( "name", new String[] { "Fritz", "Izi", "Pk" } ) )
    .add( Restrictions.disjunction()
        .add( Restrictions.isNull("age") )
    	.add( Restrictions.eq("age", new Integer(0) ) )
    	.add( Restrictions.eq("age", new Integer(1) ) )
    	.add( Restrictions.eq("age", new Integer(2) ) )
    ) )
    .list();
```

Hibernate提供了相当多的内置criterion类型(`Restrictions` 子类), 但是尤其有用的是可以允许你直接使用SQL。

```sql
List cats = sess.createCriteria(Cat.class)
    .add( Restrictions.sqlRestriction("lower({alias}.name) like lower(?)", "Fritz%", Hibernate.STRING) )
    .list();
```

`{alias}`占位符应当被替换为被查询实体的列别名。

`Property`实例是获得一个条件的另外一种途径。你可以通过调用`Property.forName()` 创建一个`Property`。

```sql
Property age = Property.forName("age");
List cats = sess.createCriteria(Cat.class)
    .add( Restrictions.disjunction()
        .add( age.isNull() )
    	.add( age.eq( new Integer(0) ) )
    	.add( age.eq( new Integer(1) ) )
    	.add( age.eq( new Integer(2) ) )
    ) )
    .add( Property.forName("name").in( new String[] { "Fritz", "Izi", "Pk" } ) )
    .list();
```

### d>结果集排序

你可以使用`org.hibernate.criterion.Order`来为查询结果排序。

```sql
List cats = sess.createCriteria(Cat.class)
    .add( Restrictions.like("name", "F%")
    .addOrder( Order.asc("name") )
    .addOrder( Order.desc("age") )
    .setMaxResults(50)
    .list();
List cats = sess.createCriteria(Cat.class)
    .add( Property.forName("name").like("F%") )
    .addOrder( Property.forName("name").asc() )
    .addOrder( Property.forName("age").desc() )
    .setMaxResults(50)
    .list();
```

### e>关联

你可以使用`createCriteria()`非常容易的在互相关联的实体间建立 约束。

```sql
List cats = sess.createCriteria(Cat.class)
    .add( Restrictions.like("name", "F%") )
    .createCriteria("kittens")
        .add( Restrictions.like("name", "F%") )
    .list();
```

注意第二个 `createCriteria()`返回一个新的 `Criteria`实例，该实例引用`kittens` 集合中的元素。

接下来，替换形态在某些情况下也是很有用的。

```sql
List cats = sess.createCriteria(Cat.class)
    .createAlias("kittens", "kt")
    .createAlias("mate", "mt")
    .add( Restrictions.eqProperty("kt.name", "mt.name") )
    .list();
```

(`createAlias()`并不创建一个新的 `Criteria`实例。)

`Cat`实例所保存的之前两次查询所返回的kittens集合是 *没有*被条件预过滤的。如果你希望只获得符合条件的kittens， 你必须使用`ResultTransformer`。

```sql
List cats = sess.createCriteria(Cat.class)
    .createCriteria("kittens", "kt")
        .add( Restrictions.eq("name", "F%") )
    .setResultTransformer(Criteria.ALIAS_TO_ENTITY_MAP)
    .list();
Iterator iter = cats.iterator();
while ( iter.hasNext() ) {
    Map map = (Map) iter.next();
    Cat cat = (Cat) map.get(Criteria.ROOT_ALIAS);
    Cat kitten = (Cat) map.get("kt");
}
```

### h>离线(detached)查询和子查询

`DetachedCriteria`类使你在一个session范围之外创建一个查询，并且可以使用任意的 `Session`来执行它。

```
DetachedCriteria query = DetachedCriteria.forClass(Cat.class)
    .add( Property.forName("sex").eq('F') );
    
Session session = ....;
Transaction txn = session.beginTransaction();
List results = query.getExecutableCriteria(session).setMaxResults(100).list();
txn.commit();
session.close();
```

`DetachedCriteria`也可以用以表示子查询。条件实例包含子查询可以通过 `Subqueries`或者`Property`获得。

```sql
DetachedCriteria avgWeight = DetachedCriteria.forClass(Cat.class)
	.setProjection( Property.forName("weight").avg() );
session.createCriteria(Cat.class)
	.add( Property.forName("weight).gt(avgWeight) )
	.list();
DetachedCriteria weights = DetachedCriteria.forClass(Cat.class)
	.setProjection( Property.forName("weight") );
session.createCriteria(Cat.class)
	.add( Subqueries.geAll("weight", weights) )
	.list();
```

甚至相互关联的子查询也是有可能的：

```sql
DetachedCriteria avgWeightForSex = DetachedCriteria.forClass(Cat.class, "cat2")
	.setProjection( Property.forName("weight").avg() )
	.add( Property.forName("cat2.sex").eqProperty("cat.sex") );
session.createCriteria(Cat.class, "cat")
	.add( Property.forName("weight).gt(avgWeightForSex) )
	.list();
```

## 3、SQL

# 十一、缓存

## 1、一级缓存

### a>简介

Hibernate 是一款全自动 ORM 框架，它会在应用程序访问数据时，自动生成 SQL 语句并执行，因此开发人员不需要自己编写 SQL 语句，但这也造成它无法像 MyBatis 一样，能够直接从 SQL 层面严格控制其执行性能以及对数据库的访问频率，所以很容易出现性能不佳的情况。

为此，Hibernate 提供了多种性能优化手段（例如 HQL、懒加载策略、抓取策略以及缓存机制），其中缓存机制是 Hibernate 重要的优化手段之一，它可以有效地减少应用程序对数据库的访问的次数，提高应用程序的运行性能。

### b>缓存

缓存是位于应用程序和永久性数据存储源（例如硬盘上的文件或者数据库）之间，用于临时存放备份数据的内存区域，通过它可以降低应用程序读写永久性数据存储源的次数，提高应用程序的运行性能。



![缓存在系统中位置](https://ningct.oss-cn-hangzhou.aliyuncs.com/105A4K04-0.png)
图1：缓存在系统中位置

 

> 注：永久性数据存储源一般包括两种，数据库和硬盘上的文件，它们都可以永久的保存数据，但本教程中的永久性数据库存储源则是特指数据库，因此在后面的教程中，我们将使用数据库来代替永久性数据存储源的书法，特此说明。

缓存具有以下特点：

- 缓存中的数据通常是数据库中数据的备份，两者中存放的数据完全一致，因此应用程序运行时，可以直接读写缓存中的数据，而不再对数据库进行访问，可以有效地降低应用程序对数据库的访问频率。
- 缓存的物理介质通常是内存，永久性数据存储源的物理介质为硬盘或磁盘，而应用程序读取内存的速度要明显高于硬盘，因此使用缓存能够有效的提高数据读写的速度，提高应用程序的性能。
- 由于应用程序可以修改（即“写”）缓存中的数据，为了保证缓存和数据库中的数据保持一致，应用程序通常会在在某些特定时刻，将缓存中的数据同步更新到数据库中。


Hibernate 也提供了缓存机制，当查询数据时，首先 Hibernate 会到缓存中查找，如果找到就直接使用，找不到时才从永久性数据存储源（通常指的是数据库）中检索，因此，把频繁使用的数据加载到缓存中，可以减少应用程序对数据库的访问频次，使应用程序的运行性能得以提升。

### c>特点

Hibernate 一级缓存是 Session 级别的缓存，它是由 Hibernate 管理的，不可卸载。

Hibernate 一级缓存是由 Session 接口实现中的一系列 Java 集合构成的，其生命周期与 Session 保持一致。

Hibernate 一级缓存中存放的数据是数据库中数据的备份，在数据库中数据以数据库记录的形式保存，而在 Hibernate 一级缓存中数据是以对象的形式存放的。

当使用 Hibernate 查询对象时，会首先从一级缓存中查找，若在一级缓存中找到了匹配的对象，则直接取出并使用；若没有在一级缓存中找到匹配的对象，则去数据库中查询对应的数据，并将查询到的数据添加到一级缓存中。由此可见，Hibernate 的一级缓存机制能够在 Session 范围内，有效的减少对数据库的访问次数，优化 Hibernate 的性能。

一旦对象被存入一级缓存中，除非用户手动清除，不然只要 Session 实例的生命周期没有结束，存放在其中的对象就会一直存在。当 Session 关闭时，Session 的生命周期结束，该 Session 所管理的一级缓存也会立即被清除；

Hibernate 一级缓存具有以下特点：

- 一级缓存是 Hibernate 自带的，默认是开启状态，无法卸载。
- Hibernate 一级缓存中只能保存持久态对象。
- Hibernate 一级缓存的生命周期与 Session 保持一致，且一级缓存是 Session 独享的，每个 Session 不能访问其他的 Session 的缓存区，Session 一旦关闭或销毁，一级缓存中的所有对象将全部丢失。
- 当通过 Session 接口提供的 save()、update()、saveOrUpdate() 和 lock() 等方法，对对象进行持久化操作时，该对象会被添加到一级缓存中。
- 当通过 Session 接口提供的 get()、load() 方法，以及 Query 接口提供的 getResultList、list() 和 iterator() 方法，查询某个对象时，会首先判断缓存中是否存在该对象，如果存在，则直接取出来使用，而不再查询数据库；反之，则去数据库查询数据，并将查询结果添加到缓存中。
- 当调用 Session 的 close() 方法时，Session 缓存会被清空。
- 一级缓存中的持久化对象具有自动更新数据库能力。
- 一级缓存是由 Hibernate 维护的，用户不能随意操作缓存内容，但用户可以通过 Hibernate 提供的方法，来管理一级缓存中的内容，如下表。



| 返回值类型 | 方法                              | 描述                                                     |
| ---------- | --------------------------------- | -------------------------------------------------------- |
| void       | clear()                           | 该方法用于清空一级缓存中的所有对象。                     |
| void       | evict(Object var1)                | 该方法用于清除一级缓存中某一个对象。                     |
| void       | flush() throws HibernateException | 该方法用于刷出缓存，使数据库与一级缓存中的数据保持一致。 |

### d>快照

## 快照区

Hibernate 的缓存机制，可以有效的减少应用程序对数据库的访问次数，但该机制有一个非常重要的前提，那就是必须确保一级缓存中的数据域与数据库的数据保持一致，为此 Hibernate 中还提供了快照技术。

Hibernate 的 Session 中，除了一级缓存外，还存在一个与一级缓存相对应的快照区。当 Hibernate 向一级缓存中存入数据（持久态对象）时，还会复制一份数据存入快照区中，使一级缓存和快照区的数据完全相同。

当事务提交时，Hibernate 会检测一级缓存中的数据和快照区的数据是否相同。若不同，则 Hibernate 会自动执行 update() 方法，将一级缓存的数据同步更新到数据库中，并更新快照区，这一过程被称为刷出缓存（flush）；反之，则不会刷出缓存。

默认情况下，Session 会在以下时间点刷出缓存：

- 当应用程序调用 Transaction 的 commit() 方法时, 该方法先刷出缓存（session.flush()），然后再向数据库提交事务（tx.commit()）;
- 当应用程序执行一些查询操作时，如果缓存中持久化对象的属性已经发生了变化，会先刷出缓存，以保证查询结果能够反映持久化对象的最新状态;
- 手动调用 Session 的 flush() 方法。

## 2、二级缓存

### a>简介

二级缓存是 SessionFactory 级别的缓存，它是属于进程范围的缓存，这一级别的缓存可以进行配置和更改，以及动态地加载和卸载，它是由 SessionFactory 负责管理的。

二级缓存与一级缓存一样，也是根据对象的 ID 加载和缓存数据的。当执行某个查询获得的结果集为实体对象时，Hibernate 就会把获得的实体对象按照 ID 加载到二级缓存中。

在访问指定的对象时，首先从一级缓存中查找，找到就直接使用，找不到则转到二级缓存中查找（必须配置和启用二级缓存）。如果在二级缓存中找到，就直接使用，否则会查询数据库，并将查询结果根据对象的 ID 放到一级缓存和二级缓存中。

b>分类

SessionFactory的缓存包括两部分：
内置缓存：使用一个Map，用于存放配置信息，预定义HQL语句等，提供给Hibernate框架自己使用，对外只读的。不能写入,也就是不能更改数据。

外置缓存：使用另一个Map，用于存放用户自定义数据。默认不开启。外置缓存hibernate只提供规范（接口），需要第三方实现类

### c>缓存插件

◆EhCache：可作为进程范围的缓存，存放数据的物理介质可以是内存或硬盘，对Hibernate的查询缓存提供了支持。
◆OSCache：可作为进程范围的缓存，存放数据的物理介质可以是内存或硬盘，提供了丰富的缓存数据过期策略，对Hibernate的查询缓存提供了支持。
◆SwarmCache：可作为群集范围内的缓存，但不支持Hibernate的查询缓存。
◆JBossCache：可作为群集范围内的缓存，支持事务型并发访问策略，对Hibernate的查询缓存提供了支持。

### d>配置二级缓存

配置Hibernate二级缓存的主要步骤：
◆选择需要使用二级缓存的持久化类，设置它的命名缓存的并发访问策略。这是最值得认真考虑的步骤。
◆选择合适的缓存插件，然后编辑该插件的配置文件。

# 十二、批处理

## 1、Session批处理

Session的save()及update()方法都会把处理的对象存放在自己的缓存中。如果通过一个Session对象来处理大量持久化对象，应该及时从缓存中清空已经处理完毕并且不会再访问的对象。具体的做法是在处理完一个对象或小批量对象后，立即调用flush()方法刷新缓存，然后再调用clear()方法情况缓存

## 2、HQL批处理

### a>插入

如果要将很多对象持久化，你必须通过经常的调用 `flush()` 以及稍后调用 `clear()` 来控制第一级缓存的大小。

```sql
Session session = sessionFactory.openSession();
Transaction tx = session.beginTransaction();
   
for ( int i=0; i<100000; i++ ) {
    Customer customer = new Customer(.....);
    session.save(customer);
    if ( i % 20 == 0 ) { //20, same as the JDBC batch size //20,与JDBC批量设置相同
        //flush a batch of inserts and release memory:
        //将本批插入的对象立即写入数据库并释放内存
        session.flush();
        session.clear();
    }
}
   
tx.commit();
session.close();
```

### b>更新

此方法同样适用于检索和更新数据。此外，在进行会返回很多行数据的查询时， 你需要使用 `scroll()` 方法以便充分利用服务器端游标所带来的好处。

```sql
Session session = sessionFactory.openSession();
Transaction tx = session.beginTransaction();
   
ScrollableResults customers = session.getNamedQuery("GetCustomers")
    .setCacheMode(CacheMode.IGNORE)
    .scroll(ScrollMode.FORWARD_ONLY);
int count=0;
while ( customers.next() ) {
    Customer customer = (Customer) customers.get(0);
    customer.updateStuff(...);
    if ( ++count % 20 == 0 ) {
        //flush a batch of updates and release memory:
        session.flush();
        session.clear();
    }
}
   
tx.commit();
session.close();
```

### c>StatelessSession

`StatelessSession` 接口定义的`insert(), update()` 和 `delete()`操作是直接的数据库行级别操作，其结果是立刻执行一条`INSERT, UPDATE` 或 `DELETE` 语句。因此，它们的语义和`Session` 接口定义的`save(), saveOrUpdate()` 和`delete()` 操作有很大的不同。

# 十三、注解

## 1、Hibernate Annotation 的环境设置

首先，我们必须确保使用的是 JDK 5.0，否则需要将 JDK 升级到 JDK 5.0 以利用对注解的本机支持。

其次，我们需要安装 Hibernate 3.x 注解分发包，可从 sourceforge 获得：（下载 Hibernate Annotation）并复制 `hibernate-annotations.jar`、`lib/hibernate-comons-annotations.jar` 和 `lib/ejb3-persistence.jar` 从 Hibernate Annotations 导入到我们的 CLASSPATH。

------

## 2、带注解的类示例

正如我们上面提到的，在使用 Hibernate Annotation 时，所有元数据都与代码一起被合并到 POJO java 文件中，这有助于用户在开发过程中同时理解表结构和 POJO。

考虑我们将使用以下 employee 表来存储我们的对象

```sql
create table employee (
   id INT NOT NULL auto_increment,
   first_name VARCHAR(20) default NULL,
   last_name  VARCHAR(20) default NULL,
   salary     INT  default NULL,
   PRIMARY KEY (id)
);
```

以下是带有注解的 Employee 类与定义的 employee 表映射对象的映射

```java
import javax.persistence.*;

@Entity
@Table(name = "employee")
public class Employee {
   @Id @GeneratedValue
   @Column(name = "id")
   private int id;

   @Column(name = "first_name")
   private String firstName;

   @Column(name = "last_name")
   private String lastName;

   @Column(name = "salary")
   private int salary;  

   public Employee() {}
   
   public int getId() {
      return id;
   }
   
   public void setId( int id ) {
      this.id = id;
   }
   
   public String getFirstName() {
      return firstName;
   }
   
   public void setFirstName( String first_name ) {
      this.firstName = first_name;
   }
   
   public String getLastName() {
      return lastName;
   }
   
   public void setLastName( String last_name ) {
      this.lastName = last_name;
   }
   
   public int getSalary() {
      return salary;
   }
   
   public void setSalary( int salary ) {
      this.salary = salary;
   }
}
```

Hibernate 检测到 `@Id` 注解在一个字段上，并假定它应该在运行时直接通过字段访问对象的属性。 如果在 `getId()` 方法上放置了 `@Id` 注释，则默认情况下我们将启用通过 getter 和 setter 方法访问属性。 因此，所有其他注解也按照所选策略放置在字段或 getter 方法上。

以下部分将解释上述类中使用的注解。

------

## 3、@Entity 注解

EJB 3 标准注解包含在 `javax.persistence` 包中，因此我们首先导入这个包。 其次，我们对 Employee 类使用了`@Entity` 注解，它将这个类标记为一个实体 bean，因此它必须有一个至少在受保护范围内可见的无参数构造函数。

## 4、@Table 注解

`@Table` 注解允许我们指定将用于将实体持久保存在数据库中的表的详细信息。

`@Table` 注解提供了四个属性，允许我们覆盖表的名称、目录和模式，并对表中的列强制执行唯一约束。 目前，我们只使用表名，即 employee。

## 5、@Id 和 @GeneratedValue 注解

每个实体 bean 都有一个主键，我们可以使用 `@Id` 注解在类上对其进行注解。 主键可以是单个字段或多个字段的组合，具体取决于表结构。

默认情况下，`@Id` 注解将自动确定要使用的最合适的主键生成策略，但我们可以通过应用 `@GeneratedValue` 注解来覆盖它，它带有两个参数 strategy 和 generator，我不打算在这里讨论，所以 让我们只使用默认的密钥生成策略。 让 Hibernate 确定要使用的生成器类型使我们的代码在不同数据库之间可移植。

## 6、@Column 注解

@Column 注解用于指定字段或属性将映射到的列的详细信息。 我们可以使用具有以下最常用属性的列注解

- **name** 属性允许明确指定列的名称。
- **length** 属性允许用于映射值的列的大小，特别是对于 String 值。
- 可空属性允许在生成模式时将该列标记为 NOT NULL。
- **unique** 属性允许将列标记为仅包含唯一值。





