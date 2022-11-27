[TOC]



# 一、简介

Spring Data : Spring 的一个子项目。用于简化数据库访问，支持NoSQL 和 关系数据存储。其主要目标是使数据库的访问变得方便快捷。

SpringData 项目所支持 NoSQL 存储：

+ MongoDB （文档数据库）

+ Neo4j（图形数据库）

+ Redis（键/值存储）

+ Hbase（列族数据库）

  

SpringData 项目所支持的关系数据存储技术：

+ JDBC
+ JPA

JPA Spring Data : 致力于减少数据访问层 (DAO) 的开发量。开发者唯一要做的，就只是声明持久层的接口，其他都交给 Spring Data JPA 来帮你完成！

> 框架怎么可能代替开发者实现业务逻辑呢？比如：当有一个 UserDao.findUserById()  这样一个方法声明，大致应该能判断出这是根据给定条件的 ID 查询出满足条件的 User  对象。Spring Data JPA 做的便是规范方法的名字，根据符合规范的名字来确定方法需要实现什么样的逻辑。

# 二、开发步骤

使用Spring Data JPA进行持久层开发需要的四个步骤：

 - 配置Spring 整合 JPA

 - 在Spring 配置文件中配置Spring Data ，让Spring 为声明的接口创建代理对象。配置了<jpa:repositories>后，Spring 初始化容器将会扫描base-package 指定的包目录及其子目录，为继承Repository 或其子接口的接口创建代理对象，并将代理对象注册为Spring Bean ,业务层便可以通过Spring自动封装的特性来直接使用该对象。

 - 声明持久层的接口，该接口继承 Repository ,Repository是一个标记型接口，它不包含任何方法，如必要，Spring Data 可实现Repository其他子接口，其中定义了一些常用的增删改查，以及分页相关的方法。

 - 在接口中声明需要的方法。Spring Data 将根据给定的策略（具体策略稍后讲解）来为其生成实现代码。

   ```java
   //根据 lastName 来获取对应的 Person
   	Person getByLastName(String lastName);
   ```

   

# 三、Repository 接口

## 1、概述

Repository 接口是 Spring Data 的一个核心接口，它不提供任何方法，开发者需要在自己定义的接口中声明需要的方法 

Spring Data可以让我们只定义接口，只要遵循 Spring Data的规范，就无需写实现类。


与继承 Repository 等价的一种方式，就是在持久层接口上使用 @RepositoryDefinition 注解，并为其指定 domainClass 和 idClass 属性

## 2、继承体系

基础的 Repository 提供了最基本的数据访问功能，其几个子接口则扩展了一些功能。它们的继承关系如下： 

+ Repository： 仅仅是一个标识，表明任何继承它的均为仓库接口类
+ CrudRepository： 继承 Repository，实现了一组 CRUD 相关的方法 
+ PagingAndSortingRepository： 继承 CrudRepository，实现了一组分页排序相关的方法 
+ JpaRepository： 继承 PagingAndSortingRepository，实现一组 JPA 规范相关的方法 
  自定义的 XxxxRepository 需要继承 JpaRepository，这样的 XxxxRepository 接口就具备了通用的数据访问控制层的能力。

JpaSpecificationExecutor： 不属于Repository体系，实现一组 JPA Criteria 查询相关的方法 

## 3、方法命名规范

在 Repository 子接口中声明方法

 * 不是随便声明的. 而需要符合一定的规范
 * 查询方法以 find | read | get 开头
 * 涉及条件查询时，条件的属性用条件关键字连接
 * 要注意的是：条件属性以首字母大写。
 * 支持属性的级联查询. 若当前类有符合条件的属性, 则优先使用, 而不使用级联属性. 
 * 若需要使用级联属性, 则属性之间使用 _ 进行连接. 

 spring data 支持的关键字

| 关键字            | 方法命名                       | sql where字句              |
| ----------------- | ------------------------------ | -------------------------- |
| And               | findByNameAndPwd               | where name= ? and pwd =?   |
| Or                | findByNameOrSex                | where name= ? or sex=?     |
| Is,Equals         | findById,findByIdEquals        | where id= ?                |
| Between           | findByIdBetween                | where id between ? and ?   |
| LessThan          | findByIdLessThan               | where id < ?               |
| LessThanEquals    | findByIdLessThanEquals         | where id <= ?              |
| GreaterThan       | findByIdGreaterThan            | where id > ?               |
| GreaterThanEquals | findByIdGreaterThanEquals      | where id > = ?             |
| After             | findByIdAfter                  | where id > ?               |
| Before            | findByIdBefore                 | where id < ?               |
| IsNull            | findByNameIsNull               | where name is null         |
| isNotNull,NotNull | findByNameNotNull              | where name is not null     |
| Like              | findByNameLike                 | where name like ?          |
| NotLike           | findByNameNotLike              | where name not like ?      |
| StartingWith      | findByNameStartingWith         | where name like '?%'       |
| EndingWith        | findByNameEndingWith           | where name like '%?'       |
| Containing        | findByNameContaining           | where name like '%?%'      |
| OrderBy           | findByIdOrderByXDesc           | where id=? order by x desc |
| Not               | findByNameNot                  | where name <> ?            |
| In                | findByIdIn(Collection<?> c)    | where id in (?)            |
| NotIn             | findByIdNotIn(Collection<?> c) | where id not  in (?)       |
| True              | findByAaaTue                   | where aaa = true           |
| False             | findByAaaFalse                 | where aaa = false          |
| IgnoreCase        | findByNameIgnoreCase           | where UPPER(name)=UPPER(?) |

# 四、@Query 注解

## 1、概述

这种查询可以声明在 Repository 方法中，摆脱像命名查询那样的约束，将查询直接在相应的接口方法中声明，结构更为清晰，这是 Spring data 的特有实现。

```java
//查询 id 值最大的那个 Person
	//使用 @Query 注解可以自定义 JPQL 语句以实现更灵活的查询
	@Query("SELECT p FROM Person p WHERE p.id = (SELECT max(p2.id) FROM Person p2)")
	Person getMaxIdPerson();
```

## 2、占位符

```java
//为 @Query 注解传递参数的方式1: 使用占位符. 
	@Query("SELECT p FROM Person p WHERE p.lastName = ?1 AND p.email = ?2")
	List<Person> testQueryAnnotationParams1(String lastName, String email);
```



## 3、命名参数

```java
//为 @Query 注解传递参数的方式1: 命名参数的方式. 
	@Query("SELECT p FROM Person p WHERE p.lastName = :lastName AND p.email = :email")
	List<Person> testQueryAnnotationParams2(@Param("email") String email, @Param("lastName") String lastName);
```



## 4、模糊查询

```java
//SpringData 允许在占位符上添加 %%. 
	@Query("SELECT p FROM Person p WHERE p.lastName LIKE %?1% OR p.email LIKE %?2%")
	List<Person> testQueryAnnotationLikeParam(String lastName, String email);
```

## 5、本地sql

```java
//设置 nativeQuery=true 即可以使用原生的 SQL 查询
	@Query(value="SELECT count(id) FROM jpa_persons", nativeQuery=true)
	long getTotalCount();
```

## 6、修改

```java
//可以通过自定义的 JPQL 完成 UPDATE 和 DELETE 操作. 注意: JPQL 不支持使用 INSERT
	//在 @Query 注解中编写 JPQL 语句, 但必须使用 @Modifying 进行修饰. 以通知 SpringData, 这是一个 UPDATE 或 DELETE 操作
	//UPDATE 或 DELETE 操作需要使用事务, 此时需要定义 Service 层. 在 Service 层的方法上添加事务操作. 
	//默认情况下, SpringData 的每个方法上有事务, 但都是一个只读事务. 他们不能完成修改操作!
	@Modifying
	@Query("UPDATE Person p SET p.email = :email WHERE id = :id")
	void updatePersonEmail(@Param("id") Integer id, @Param("email") String email);
```



# 五、事务

Spring Data 提供了默认的事务处理方式，即所有的查询均声明为只读事务。

对于自定义的方法，如需改变 Spring Data 提供的事务默认方式，可以在方法上注解 @Transactional 声明 

进行多个 Repository 操作时，也应该使它们在同一个事务中处理，按照分层架构的思想，这部分属于业务逻辑层，因此，需要在 Service 层实现对多个 Repository 的调用，并在相应的方法上声明事务。 

# 六、核心API

## 1、CrudRepository

CrudRepository 接口提供了最基本的对实体类的添删改查操作 

| save(T entity)                         | 保存单个实体            |
| -------------------------------------- | ----------------------- |
| save(Iterable<? extends T> entities)   | 保存集合                |
| findOne(ID id)                         | 根据id查找实体          |
| exists(ID id)                          | 根据id判断实体是否存在  |
| findAll()                              | 查询所有实体,不用或慎用 |
| count()                                | 查询实体数量            |
| delete(ID id)                          | 根据Id删除实体          |
| delete(T entity)                       | 删除一个实体            |
| delete(Iterable<? extends T> entities) | 删除一个实体的集合      |
| deleteAll()                            | 删除所有实体,不用或慎用 |

## 2、PagingAndSortingRepository

该接口提供了分页与排序功能 

+ Iterable<T> findAll(Sort sort); //排序 
+ Page<T> findAll(Pageable pageable); //分页查询（含排序功能） 

## 3、JpaRepository

该接口提供了JPA的相关功能 

+ List<T> findAll(); //查找所有实体 
+ List<T> findAll(Sort sort); //排序、查找所有实体 
+ List<T> save(Iterable<? extends T> entities);//保存集合 
+ void flush();//执行缓存与数据库同步 
+ T saveAndFlush(T entity);//强制执行持久化 
+ void deleteInBatch(Iterable<T> entities);//删除一个实体集合 

## 4、JpaSpecificationExecutor

不属于Repository体系，实现一组 JPA Criteria 查询相关的方法 

![image-20220630172547431](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220630172547431.png)

Specification：封装  JPA Criteria 查询条件。通常使用匿名内部类的方式来创建该接口的对象

# 七、自定义 Repository 方法

步骤：

+ 定义一个接口: 声明要添加的, 并自实现的方法
+ 提供该接口的实现类: 类名需在要声明的 Repository 后添加 Impl, 并实现方法
+ 声明 Repository 接口, 并继承 1) 声明的接口
+ 使用

> 注意: 默认情况下, Spring Data 会在 base-package 中查找 "接口名Impl" 作为实现类。
>
> 也可以通过　repository-impl-postfix　声明后缀.