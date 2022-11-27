[TOC]



# 一、简介

## 1、ORM

ORM（Object-Relational Mapping） 表示对象关系映射。在面向对象的软件开发中，通过ORM，就可以把对象映射到关系型数据库中。只要有一套程序能够做到建立对象与数据库的关联，操作对象就可以直接操作数据库数据，就可以说这套程序实现了ORM对象关系映射

简单的说：ORM就是建立实体类和数据库表之间的关系，从而达到操作实体类就相当于操作数据库表的目的。

主要目的： 操作实体类就相当于操作数据库表

建立两个映射关系：

```java
          实体类和表的映射关系
 
     实体类中属性和表中字段的映射关系
```
当实现一个应用程序时（不使用O/R Mapping），我们可能会写特别多数据访问层的代码，从数据库保存数据、修改数据、删除数据，而这些代码都是重复的。而使用ORM则会大大减少重复性代码。对象关系映射（Object Relational Mapping，简称ORM），主要实现程序对象到关系数据库数据的映射。

实现ORM的框架：Mybatis,Hirbernate

## 2、Hibernate

Hibernate是一个开放源代码的对象关系映射框架

它对JDBC进行了非常轻量级的对象封装，

它将POJO（java实体类对象）与数据库表建立映射关系，是一个全自动的orm框架

hibernate可以自动生成SQL语句，自动执行，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。

### 3、jpa

JPA的全称是Java Persistence API， 即Java 持久化API，是SUN公司推出的一套基于ORM的规范，内部是由一系列的接口和抽象类构成。

JPA通过JDK 5.0注解描述对象－关系表的映射关系，并将运行期的实体对象持久化到数据库中。

JPA是一套规范，实现JPA规范，内部由接口和抽象类组成

JDBC规范和JPA规范比较

![img](https://ningct.oss-cn-hangzhou.aliyuncs.com/e34581ef159947aa33ae754f7e0772e8.png)

# 二、环境搭建

## 1、创建maven工程

IDE：idea 2022.1

构建工具：maven3.8.6

jdk: 1.8

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220629171735274.png" alt="image-20220629171735274" style="zoom:50%;" />

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220623174509387.png" style="zoom: 50%;" />

## 2、配置pom.xml

```xml
<dependencies>
    <!-- junit -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>

    <!-- hibernate对jpa的支持包 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-entitymanager</artifactId>
        <version>5.6.8.Final</version>
    </dependency>

    <!-- c3p0 -->
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-c3p0</artifactId>
        <version>6.1.0.Final</version>
    </dependency>

    <!-- log日志 -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>

    <!-- Mysql and MariaDB -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.29</version>
    </dependency>
</dependencies>
```

## 3、添加jpa核心配置文件

![image-20220629172057760](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220629172057760.png)

```xml
<persistence-unit name="my_jpa" transaction-type="RESOURCE_LOCAL">
    <!--指定需要扫描的类，JPA统一管理，默认扫描@Entity 注解的类-->
    
    <!--jpa的实现方式-->
    <!--数据库信息-->
    <!--可选配置，配置jpa实现方的配置信息-->
</persistence-unit>
```

# 三、主键生成策略

通过annotation（注解）来映射hibernate实体的,基于annotation的hibernate主键标识为@Id, 其生成规则由@GeneratedValue（生成值）设定的.这里的@id和@GeneratedValue都是JPA的标准用法。

JPA提供的四种标准用法为TABLE,SEQUENCE（sequence）,IDENTITY(identity),AUTO(auto)。

## 1、IDENTITY

主键由数据库自动生成（自动增长型）

```java
@Id  //声明私有属性为主键
@GeneratedValue(strategy= GenerationType.IDENTITY) //配置主键的生成策略
@Column(name = "cust_id")
private Long custId;
```

## 2、SEQUENCE

根据底层数据库的序列来生成主键，条件是数据库支持序列

```java
@Id  
@GeneratedValue(strategy = GenerationType.SEQUENCE,generator="payablemoney_seq")  
@SequenceGenerator(name="payablemoney_seq", sequenceName="seq_payment")  
private Long custId;
```

 

```java
//@SequenceGenerator源码中的定义
@Target({TYPE, METHOD, FIELD})   
@Retention(RUNTIME)  
public @interface SequenceGenerator {  
   //表示该表主键生成策略的名称，它被引用在@GeneratedValue中设置的“generator”值中
   String name();  
   //属性表示生成策略用到的数据库序列名称。
   String sequenceName() default "";  
   //表示主键初识值，默认为0
   int initialValue() default 0;  
   //表示每次主键值增加的大小，例如设置1，则表示每次插入新记录后自动加1，默认为50
   int allocationSize() default 50;  
}
```

## 3、AUTO

主键由程序控制

```java
@Id  
@GeneratedValue(strategy = GenerationType.AUTO)  
private Long custId;
```

## 4、TABLE

使用一个特定的数据库表格来保存主键

```java
@Id  
@GeneratedValue(strategy = GenerationType.TABLE, generator="payablemoney_gen")  
@TableGenerator(name = "pk_gen",  
    table="tb_generator",  
    pkColumnName="gen_name",  
    valueColumnName="gen_value",  
    pkColumnValue="PAYABLEMOENY_PK",  
    allocationSize=1  
) 
private Long custId;


//@TableGenerator的定义：
    @Target({TYPE, METHOD, FIELD})   
    @Retention(RUNTIME)  
    public @interface TableGenerator {  
      //表示该表主键生成策略的名称，它被引用在@GeneratedValue中设置的“generator”值中
      String name();  
      //表示表生成策略所持久化的表名，例如，这里表使用的是数据库中的“tb_generator”。
      String table() default "";  
      //catalog和schema具体指定表所在的目录名或是数据库名
      String catalog() default "";  
      String schema() default "";  
      //属性的值表示在持久化表中，该主键生成策略所对应键值的名称。例如在“tb_generator”中将“gen_name”作为主键的键值
      String pkColumnName() default "";  
      //属性的值表示在持久化表中，该主键当前所生成的值，它的值将会随着每次创建累加。例如，在“tb_generator”中将“gen_value”作为主键的值 
      String valueColumnName() default "";  
      //属性的值表示在持久化表中，该生成策略所对应的主键。例如在“tb_generator”表中，将“gen_name”的值为“CUSTOMER_PK”。 
      String pkColumnValue() default "";  
      //表示主键初识值，默认为0。 
      int initialValue() default 0;  
      //表示每次主键值增加的大小，例如设置成1，则表示每次创建新记录后自动加1，默认为50。
      int allocationSize() default 50;  
      UniqueConstraint[] uniqueConstraints() default {};  
    } 
//这里应用表tb_generator，定义为 ：
CREATE TABLE  tb_generator (  
  id NUMBER NOT NULL,  
  gen_name VARCHAR2(255) NOT NULL,  
  gen_value NUMBER NOT NULL,  
  PRIMARY KEY(id)  
)
```

# 四、核心API

## 1、 Persistence对象

Persistence对象主要作用是用于获取EntityManagerFactory对象的 。通过调用该类的createEntityManagerFactory静态方法，根据配置文件中持久化单元名称创建EntityManagerFactory。

```java
  @Test
    public void test(){
        /**
         * 1、创建EntityManagerFactory
         */
        String unitName="myJpa";
        EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory(unitName);
```

## 2、EntityManagerFactory

EntityManagerFactory 接口主要用来创建 EntityManager 实例

```java
  /**
         * 2、创建实体管理类
         * EntityManagerFactory 接口主要用来创建 EntityManager 实例
         */
        EntityManager entityManager = entityManagerFactory.createEntityManager();
```

> 注意： 由于EntityManagerFactory 是一个线程安全的对象（即多个线程访问同一个EntityManagerFactory 对象不会有线程安全问题），并且EntityManagerFactory 的创建极其浪费资源，所以在使用JPA编程时，我们可以对EntityManagerFactory的创建进行优化，只需要做到一个工程只存在一个EntityManagerFactory 即可

## 3、EntityManager

在 JPA 规范中, EntityManager是完成持久化操作的核心对象。实体类作为普通 java对象，只有在调用 EntityManager将其持久化后才会变成持久化对象。EntityManager对象在一组实体类与底层数据源之间进行 O/R 映射的管理。它可以用来管理和更新 Entity Bean, 根椐主键查找 Entity Bean, 还可以通过JPQL语句查询实体。

  GetTransaction : 获取事务对象
  Persist ： 保存操作
  Merge ： 更新操作
  Remove ： 删除操作
  Find/GetReference ： 根据id查询

```java
 /**
         * 3、entityManager相关操作
         *
         *   1.GetTransaction : 获取事务对象
         *   2.Persist（坚持） ： 保存操作
         *   3.Merge （合并）： 更新操作
         *   4.Remove（删除） ： 删除操作
         *   5.Find/GetReference（查找/获取引用） ： 根据id查询
         */
 
        //获取事务对象
        EntityTransaction transaction = entityManager.getTransaction();
        //开启事务
        transaction.begin();
        //提交事务·
        transaction.commit();
        //回滚事务
        transaction.rollback();
        
        //添加操作
        entityManager.persist();
        //更新操作
        entityManager.merge();
        //删除操作
        entityManager.remove();
        //查询操作
        entityManager.find();
```



## 4、EntityTransaction

在 JPA 规范中, EntityTransaction是完成事务操作的核心对象，对于EntityTransaction在我们的java代码中承接的功能比较简单

```java
     //获取事务对象
        EntityTransaction transaction = entityManager.getTransaction();
        //开启事务
        transaction.begin();
        //提交事务·
        transaction.commit();
        //回滚事务
        transaction.rollback();
```



# 五、工具类JpaUtil

## 1、工具类JpaUtil

```java
public class JpaUtil {
    /**
     * Jpa的实体类管理工具：相当于Hirbernate的SessionFactory
     */
    private static  EntityManagerFactory em;
 
    //静态代码块赋值
    static {
        //注意：该方法参数必须和persistence.xml中persistence-unit标签name属性取值一致
       em = Persistence.createEntityManagerFactory("myJpa");
    }
 
    /**
     * 使用管理器工厂生产一个管理器对象
     * @return   管理器对象
     */ 
    public static EntityManager getEntityManager() {
        EntityManager entityManager = em.createEntityManager();
        return entityManager;
    }
}
```



# 六、jpql

## 1、简介

JPQL（Java 持久性查询语言）是一种面向对象的查询语言，用于对持久实体执行数据库操作。JPQL 不使用数据库表，而是使用实体对象模型来操作 SQL 查询。这里 JPA 的作用是将 JPQL 转换为 SQL。因此，它为开发人员提供了一个处理 SQL 任务的简单方式。

JPQL 是实体 JavaBeans 查询语言（EJBQL）的扩展，向其添加了以下重要功能：

+ 它可以执行连接操作

+ 它可以批量更新和删除数据

+ 它可以使用排序和分组子句执行聚合函数

+ 单值和多值结果类型

  

## 2、功能特性

它是一种独立于平台的查询语言

它简单而强大

它可以用于任何类型的数据库，如：MySQL、Oracle

JPQL 查询可以静态地声明为元数据，也可以动态构建在代码中

## 3、查询

### a>语法结构

语法： `SELECT ... FROM ... [WHERE ...] [GROUP BY ... [HAVING ...]] [ORDER BY ...]`

- `FROM` 子句：通过声明一个或多个标识符变量来定义查询的范围 。 并且在 `SELECT` 和 `WHERE` 子句中可以引用这些标识符变量 。
- `WHERE` 子句：用于限制查询到的对象或值的条件表达式 。
- `GROUP BY` 子句：根据一组属性对查询结果进行分组 。
- `HAVING` 子句：配合 `GROUP BY` 子句使用 ， 以根据条件表达式进一步限制查询结果 。
- `ORDER BY` 子句：对查询结果进行排序 。

### b>查询基础

语法： `SELECT 标识符变量 FROM 实体名称 [AS] 标识符变量`

```sql
@Query("SELECT E FROM Employee E") List<Employee> selectExample();
```

### c>查询参数

JPQL 支持两种查询参数 ， 它们分别是命名参数和位置参数 。

#### 1）命名参数

语法： `:` + `自定义的参数名称`

示例：按性别和薪资范围查找雇员信息

```sql
@Query("SELECT E FROM Employee E WHERE E.sex = :sex AND E.salary > :salary")
List<Employee> selectExample(@Param("sex") String sex, @Param("salary") Double salary);
```

> 在方法的参数列表中 ， 需要使用 `@Param` 注解标注每个参数的名称 ， 使之与查询语句参数名称匹配 。

#### 2)位置参数

语法： `?` + `位置编号的数值`

示例：按姓名和性别查找雇员信息

```sql
@Query("SELECT E FROM Employee E WHERE E.sex = ?1 AND E.salary > ?2")
List<Employee> selectExample(String sex, Double salary);
```

> 在方法的参数列表中 ， 参数的顺序需要与查询语句中参数标注的编号依次对应起来 。

### d>关联查询

#### 1）单值关联

语法： `SELECT 标识符变量 FROM 实体名称 [AS] 标识符变量 JOIN 实体名称.单值关联字段 [AS] 标识符变量2 ...`

示例：按部门名称查找该部门所有的雇员信息

```sql
@Query("SELECT E FROM Employee E JOIN E.department D WHERE D.name = ?1")
List<Employee> selectExample(String deptName);
```

#### 2)多值关联

语法： `SELECT 标识符变量 FROM 实体名称 [AS] 标识符变量 JOIN 实体名称.多值关联字段 [AS] 标识符变量2 ...`

示例：查询薪资大于10000的所有雇员所属的部门信息

```sql
@Query("SELECT D FROM Department D JOIN D.employees E WHERE E.salary > 10000")
List<Department> selectExample();
```

或者： `SELECT 标识符变量 FROM 实体名称 [AS] 标识符变量, IN(实体名称.多值关联字段) [AS] 标识符变量2 ...`

```sql
@Query("SELECT D FROM Department D, IN(D.employees) E WHERE E.salary > 10000")
List<Department> selectExample();
```

### e>去重查询

语法： `SELECT DISTINCT 标识符变量 FROM 实体名称 [AS] 标识符变量 ...`

示例：查询薪资大于10000的所有雇员所属的部门信息 ， 并消除查询结果中的重复的部门

```sql
@Query("SELECT DISTINCT D FROM Department D JOIN D.employees E WHERE E.salary > 10000")
List<Department> selectExample();
```

### f>模糊查询

| 表达式              | 匹配  | 不匹配 |
| ------------------- | ----- | ------ |
| E.name LIKE ‘张%’   | 张三  | 小张伟 |
| E.name LIKE ‘张_’   | 张三  | 张三丰 |
| E.name LIKE ‘张\_%’ | 张_三 | 张三   |

示例：查询张性的所有雇员

```sql
@Query("SELECT E FROM Employee E WHERE E.name LIKE '张%'")
List<Employee> selectExample();
```

### g>空集合查询

通过使用关键字 `IS [NOT] EMPTY` 来查找关联的属性集合的值为空的记录 。

示例：查找尚无雇员的所有部门

```sql
@Query("SELECT D FROM Department D WHERE D.employees IS EMPTY")
List<Department> selectExample();
```

### h>构造器

查询结果的类型如果不是持久化的实体类 ， 必须使用该类的完全限定名 。

语法： `SELECT NEW 类的完全限定名(参数1, 参数2, ...) ...`

示例：查询所有的雇员信息

```java
package org.fanlychie.model;

public class SimpleEmployee{

 private String name;

 private Sex sex;

 public SimpleEmployee(String name, Sex sex){
 this.name = name;
 this.sex = sex;
 }

 // getters and setters
 
}
@Query("SELECT NEW org.fanlychie.model.SimpleEmployee(E.name, E.sex) FROM Employee E")
List<SimpleEmployee> selectExample();
```

## 4、更新

示例：更新某个雇员的婚姻状态和薪资信息

```sql
@Modifying
@Transactional
@Query("UPDATE Employee SET married = ?2, salary = ?3 WHERE id = ?1")
int updateExample(Long id, Boolean married, Double salary);
```

`@Query` 无法进行 `DML` （Data Manipulation Language 数据操控语言 ， 主要语句有 `INSERT` 、 `DELETE` 、 `UPDATE` ）操作 ， 如需更新数据库表的数据需要标注 `@Modifying` 注解 ， 并且需要事务的支持 `@Transactional` 。

## 5、更新

示例：删除没有雇员的部门信息

```sql
@Modifying
@Transactional
@Query("DELETE FROM Department D WHERE D.employees IS EMPTY")
int deleteExample();
```



# 七、关系映射

## 1、单向多对一

### a>@ManyToOne

是属性或方法级别的注解，用于定义源实体与目标实体是多对一的关系
属性：

+ `targetEntity`：源实体关联的目标实体类型，默认是该成员属性对应的类型，可以缺省
+ `cascade`：定义源实体和关联的目标实体间的级联关系。默认没有级联操作。可选值有：
  + `CascadeType.PERSIST`：级联新建。若保存实体时，数据库中没有与该实体相关联的实体的那条记录，会在保存实体的同时保存与之相关联的实体
  + `CascadeType.REMOVE`：级联删除。删除当前实体时，与它有映射关系的实体也会跟着被删除
  + `CascadeType.REFRESH`：级联刷新。在更新前重新获取数据。
    + 使用场景：你先获取了数据，但是在保存时数据库的数据被修改了，这时候就需要重新获取一次数据(refresh)，然后执行update操作
  + `CascadeType.MERGE`：级联更新。当当前实体的数据改变，会相应地更新关联的实体的数据
  + `CascadeType.DETACH`：级联脱管/游离操作。删除实体因为有外键无法删除时，撤销所有相关的外键关联，然后删除
  + `CascadeType.ALL`：（包含以上五项）
+ `fetch`：定义关联的目标实体的数据的加载方式。
  + `FetchType.LAZY`(懒加载) | `FetchType.EAGER`(立即加载，默认)
+ `optional`：源实体关联的目标实体是否允许为 null，默认为 true

### b>@JoinColumn

该注解用于定义外键列，只能标注在实体类型的成员属性或方法上，如果没有声明，则使用该注解的默认值

属性值：与 @Column 注解相类似，但是没有了 length、precision、scale，特有属性：

+ `referencedColumnName`：指定要关联哪一列为外键
+ `foreignKey`：没找到资料，这是源码中 Java doc 中的注释
  + The foreign key constraint specification for the join column. This is used only if table generation is in effect.  Default is provider defined.
    有道词典的翻译：联接列的外键约束规范。只有在表生成生效时才使用此方法。默认是提供程序定义的。
  + 其值的类型是：@ForeignKey，这个注解的属性
    + name：外键名

### c>操作注意

+ `保存`：建议先保存一的一方，再保存多的一方，否则会多执行update语句，因为是一的一方在维护关联关系
+ `查询`：因为JPA默认是立即加载，所以在查询多的一方的时候，默认会使用左外连接
+ `修改`：默认是可以级联修改的
+ `删除`：多的一方可以直接删除，但是一的一方因为有外键约束，所以不能删除

## 2、单向一对多

### a>@OneToMany

- 与 @ManyToOne类似，但是没有`optional`
- 特有属性：
  - `mappedBy`：用在双向关联中。如果关系是双向的，则需定义此参数。用于标注谁来维护外键
  - `orphanRemoval`：
    - 当**源实体**关联的**目标实体**被断开时，是否自动删除断开的实例（在数据库中表现为删除表示该实例的行记录），默认为 false。

### b>操作注意

`保存`：因为是一的一方在维护关联关系，所以在保存时，一定会多执行update语句

`查询`：`FetchType fetch() default LAZY`默认使用懒加载，发送多条sql，可修改为立即加载

`修改`：默认是可以级联修改的

`删除`：若删除**一**的一端，默认先将关联外键置空，然后删除记录。可以通过修改@OneToMany的`cascade`改变

## 3、双向多对一

### a>使用注解

- 一对多和多对一结合即可

### b>操作注意

1. 保存：双向一对多双方都在维护外键关系，所以在保存时，如果先保存一的一方，会执行n次update，先保存多的一方，会执行2n次update
   - 我们可以设置一的一方不维护关联关系，通过`@OneToMany`的`mappedBy`来设置有哪个属性来维护

## 4、双向一对一

### a>使用注解

- @OneToOne
- @JoinColumn

### b>操作注意

一对一关系需要在外键列添加唯一约束

建议设置一方放弃维护关联关系

保存：建议先保存没有外键的一方，再保存有外键的一方，这样不会多出update操作

查询：

1. 若获取维护关联关系（有外键）的一方
   - 默认会通过左外连接获取，即：默认是立即加载
   - 若修改为懒加载，会发送两次sql语句，但是获取到的关联对象是代理对象
2. 若获取不维护关联关系（没有外键）的一方
   - 默认会通过左外连接获取，即：默认是立即加载
   - 改为懒加载以后，会发现，他发了两次sql语句，同样把关联对象查出来了，所以，修改它的加载策略并没有什么意义

## 5、双向多对多

### a>使用注解

- @ManyToMany：属性还是那些

- @JoinTable：与@Table类似，但是多了下列属性
  - joinColumns：表示表中的外键列，该外键参照**源实体（本对象）**的主键。
  - inverseJoinColumns：与joinColumns类似，但是该外键参照**目标实体（关联对象）**的主键。
  - foreignKey：用于生成表时定义 `joinColumns` 参数的外键约束
  - inverseForeignKey：用于生成表时定义 `inverseJoinColumns` 参数的外键约束

### b>操作注意

- 多对多必须有一方放弃维护外键关系
- 多对多的**级联删除**要**慎重考虑**

