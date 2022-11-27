[TOC]



# 一、Spring 框架概述

## 1、spring描述

+ Spring 是轻量级的开源的 JavaEE 框架 
+ Spring 可以解决企业应用开发的复杂性 

## 2、Spring 核心部分：IOC 和 Aop 

### a、IOC

控制反转，把创建对象过程交给 Spring 进行管理 

### b、Aop

面向切面，不修改源代码进行功能增强 

## 3、Spring 特点 

+ 方便解耦，简化开发 
+ Aop 编程支持 
+ 方便程序测试 
+ 方便和其他框架进行整合 
+ 方便进行事务操作 
+ 降低 API 开发难度

## 4、入门案例

### a、下载Spring5

#### (1)、使用Spring最新稳定版本 5.2.6

![image-20220707173752121](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707173752121.png)

#### (2)、下载地址

https://repo.spring.io/release/org/springframework/spring/

![image-20220707173855829](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707173855829.png)

#### (3)下载内容

![image-20220707173921893](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707173921893.png)

### b、创建Java工程

![image-20220707174020253](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707174020253.png)

![image-20220707174029207](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707174029207.png)

![image-20220707174040152](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707174040152.png)

### c、导入Spring5相关jar包

![image-20220707174108793](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707174108793.png)

![image-20220707174120533](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707174120533.png)

![image-20220707174135002](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707174135002.png)

### d、创建普通类，在这个类创建普通方法

```java
public class User { 
    public void add() { System.out.println("add......"); 
                      }
}
```

### e、创建Spring配置文件，在配置文件配置创建的对象

> Spring配置文件使用xml格式

![image-20220707174357957](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707174357957.png)

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd"> 
    <!--配置User对象创建--> 
    <bean id="user" class="com.atguigu.spring5.User"></bean> 
</beans>
```

### f、进行测试代码编写

```java
@Test public void testAdd() { 
    //1 加载spring配置文件 
    ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml"); 
    //2 获取配置创建的对象 
    User user = context.getBean("user", User.class); 
    System.out.println(user); user.add();
}
```



# 二、IOC 容器 

## 1、IOC概念原理

### a、什么是IOC

+ 控制反转，把对象创建和对象之间的调用过程，交给Spring进行管理
+ 使用IOC目的：为了耦合度降低
+ 做入门案例就是IOC实现

### b、IOC底层原理

+ xml解析
+ 工厂模式
+ 反射

### c、画图讲解IOC底层原理

![image-20220707175440021](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707175440021.png)

## 2、IOC 接口（BeanFactory）

IOC思想基于IOC容器完成，IOC容器底层就是对象工厂

### a、Spring提供IOC容器实现两种方式：（两个接口）

#### (1)、BeanFactory

IOC容器基本实现，是Spring内部的使用接口，不提供开发人员进行使用

> 加载配置文件时候不会创建对象，在获取对象（使用）才去创建对象

#### (2)、ApplicationContext

BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员进行使用

> 加载配置文件时候就会把在配置文件对象进行创建

### b、ApplicationContext接口实现类

![image-20220707175835776](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707175835776.png)

## 3、IOC 操作 Bean 管理

### a、概念

#### (1)、什么是Bean管理

+ Bean管理指的是两个操作
+ Spring创建对象
+ Spirng注入属性

#### (2)、Bean管理操作有两种方式

+ 基于xml配置文件方式实现
+ 基于注解方式实现

### b、基于 xml

#### (1)、基于xml方式创建对象

在spring配置文件中，使用bean标签，标签里面添加对应属性，就可以实现对象创建

![image-20220707180334257](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707180334257.png)

#### (2)、bean标签常用属性

* id属性：唯一标识
* class属性：类全路径（包类路径）

#### (3)、创建对象方式

默认执行无参数构造方法完成对象创建

#### (4)、属性注入方式

##### [1]、使用set方法进行注入

###### <1>创建类，定义属性和对应的set方法 

```java
/** * 演示使用set方法进行注入属性 */ 
public class Book { 
	//创建属性 private String bname; 
	private String bauthor; 
	//创建属性对应的set方法 
	public void setBname(String bname) { 
		this.bname = bname; 
	} 
	public void setBauthor(String bauthor) { 
		this.bauthor = bauthor; 
	} 
}
```

###### <2>、在spring配置文件配置对象创建，配置属性注入

```xml
<!--2 set方法注入属性--> 
<bean id="book" class="com.atguigu.spring5.Book"> 
<!--使用property完成属性注入 name：类里面属性名称 value：向属性注入的值 --> 
<property name="bname" value="易筋经"></property> 
<property name="bauthor" value="达摩老祖"></property> 
</bean>
```

##### [2]、使用有参数构造进行注入

###### <1>、创建类，定义属性，创建属性对应有参数构造方法

```java
/** * 使用有参数构造注入
*/ 
public class Orders { 
	//属性 private String oname; 
	private String address; 
	//有参数构造 
	public Orders(String oname,String address) { 
		this.oname = oname; 
    	this.address = address; 
	} 
}
```

###### <2>、在spring配置文件中进行配置

```xml
 <!--3 有参数构造注入属性--> 
 <bean id="orders" class="com.atguigu.spring5.Orders"> 
 	<constructor-arg name="oname" value="电脑"></constructor-arg> 
   	<constructor-arg name="address" value="China"></constructor-arg> 
 </bean>
```

##### [3]、p名称空间注入（了解）

###### <1>、添加p名称空间在配置文件中

![image-20220707181434825](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707181434825.png)

###### <2>、进行属性注入

```xml
<!--2 set方法注入属性--> 
<bean id="book" class="com.atguigu.spring5.Book" p:bname="九阳神功" p:bauthor="无名氏"></bean>
```

#### (5)、注入其他类型属性

##### [1]、字面量

+ null值 

  ```xml
  <!--null值--> <property name="address"> <null/> </property>
  ```

+ 属性值包含特殊符号 

  ```xml
  <!--属性值包含特殊符号 1 把<>进行转义 &lt; &gt; 2 把带特殊符号内容写到CDATA --> 
  <property name="address"> <value><![CDATA[<<南京>>]]></value> </property>
  ```

##### [2]、外部bean

###### <1>、创建两个类 service类和dao类

###### <2>、在service调用dao里面的方法

```java
 public class UserService { 
     //创建UserDao类型属性，生成set方法 
     private UserDao userDao; 
     public void setUserDao(UserDao userDao) { 
         this.userDao = userDao; 
     } 
     public void add() { 
         System.out.println("service add..............."); 
         userDao.update(); 
     } 
 }
```

###### <3>、在spring配置文件中进行配置

```xml
<!--1 service和dao对象创建--> 
<bean id="userService" class="com.atguigu.spring5.service.UserService"> 
    <!--注入userDao对象 name属性：类里面属性名称 ref属性：创建userDao对象bean标签id值 --> 
    <property name="userDao" ref="userDaoImpl"></property> 
</bean> 

<bean id="userDaoImpl" class="com.atguigu.spring5.dao.UserDaoImpl">
</bean>
```

##### [3]、内部bean

###### <>、一对多关系：部门和员工

一个部门有多个员工，一个员工属于一个部门
部门是一，员工是多

###### <>、在实体类之间表示一对多关系，员工表示所属部门，使用对象类型属性进行表示

```java
 //部门类 
public class Dept { 
    private String dname; 
    public void setDname(String dname) { 
        this.dname = dname; 
    } 
} 
//员工类 
public class Emp {
    private String ename;
    private String gender; 
    //员工属于某一个部门，使用对象形式表示
    private Dept dept;
    public void setDept(Dept dept) {
        this.dept = dept; 
    } 
    public void setEname(String ename) { 
        this.ename = ename; 
    } 
    public void setGender(String gender) {
        this.gender = gender;
    } 
} 
```

###### <3>、在spring配置文件中进行配置

```xml
 <!--内部bean--> 
<bean id="emp" class="com.atguigu.spring5.bean.Emp">
    <!--设置两个普通属性--> 
    <property name="ename" value="lucy"></property> 
    <property name="gender" value="女"></property> 
    <!--设置对象类型属性--> 
    <property name="dept"> 
        <bean id="dept" class="com.atguigu.spring5.bean.Dept"> 
            <property name="dname" value="安保部"></property> 
        </bean> 
    </property> 
</bean>
```

#### (6)、注入集合属性

##### [1]、数组

```xml
<!--数组类型属性注入--> 
<property name="courses"> 
    <array> 
        <value>java课程</value> 
        <value>数据库课程</value> 
    </array> 
</property>
```

##### [2]、List

```xml
<!--list类型属性注入--> 
<property name="list"> 
    <list>
        <value>张三</value> 
        <value>小三</value> 
    </list> 
</property>
```

##### [3]、Map

```xml
<!--map类型属性注入--> 
<property name="maps"> 
    <map> 
        <entry key="JAVA" value="java"></entry> 
        <entry key="PHP" value="php"></entry> 
    </map> 
</property>
```

[4]、Set

```xml
<!--set类型属性注入--> 
<property name="sets"> 
    <set> 
        <value>MySQL</value> 
        <value>Redis</value> 
    </set> 
</property>
```

#### (7)、集合里面设置对象类型值

```xml
<!--创建多个course对象--> 
<bean id="course1" class="com.atguigu.spring5.collectiontype.Course"> 
    <property name="cname" value="Spring5框架"></property> 
</bean> 
<bean id="course2" class="com.atguigu.spring5.collectiontype.Course"> 
    <property name="cname" value="MyBatis框架"></property> 
</bean> 
<!--注入list集合类型，值是对象--> 
<property name="courseList"> 
    <list> 
        <ref bean="course1"></ref> 
        <ref bean="course2"></ref> 
    </list> 
</property>
```

#### (8)、把集合注入部分提取出来

##### [1]、在spring配置文件中引入名称空间

```xml
<?util <?xml version="1.0" encoding="UTF-8"?> 
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring
                           beans.xsd http://www.springframework.org/schema/util
                           http://www.springframework.org/schema/util/spring
                           util.xsd">
```

##### [2]、使用util标签完成list集合注入提取 

```xml
<!--1 提取list集合类型属性注入--> 
<util:list id="bookList">
<value>易筋经</value> 
    <value>九阴真经</value> 
    <value>九阳神功</value> 
</util:list> 
<!--2 提取list集合类型属性注入使用--> 
<bean id="book" class="com.atguigu.spring5.collectiontype.Book"> 
    <property name="list" ref="bookList"></property> 
</bean>
```

#### (9)、FactoryBean

Spring有两种类型bean，一种普通bean，另外一种工厂bean（FactoryBean）

##### [1]、普通bean

在配置文件中定义bean类型就是返回类型

##### [2]、工厂bean

在配置文件定义bean类型可以和返回类型不一样

###### <1>、 创建类，让这个类作为工厂bean，实现接口 FactoryBean

###### <2>、 实现接口里面的方法，在实现的方法中定义返回的bean类型

```java
public class MyBean implements FactoryBean<Course> { 
    //定义返回bean 
    @Override 
    public Course getObject() throws Exception { 
        Course course = new Course(); 
        course.setCname("abc"); 
        return course; 
    } 
    @Override 
    public Class<?> getObjectType() { 
        return null; 
    } 
    @Override 
    public boolean isSingleton() { 
        return false; 
    } 
}
```

###### <3>、配置文件

```xml
<bean id="myBean" class="com.atguigu.spring5.factorybean.MyBean"> </bean>
```

###### <4>、test

```java
@Test 
public void test3() { 
    ApplicationContext context = new ClassPathXmlApplicationContext("bean3.xml"); 
    Course course = context.getBean("myBean", Course.class); 
    System.out.println(course); 
}
```

#### (10)、bean 作用域

在Spring里面，默认情况下，bean是单实例对象

##### [1]、如何设置单实例还是多实例

###### <1>、spring配置文件bean标签属性（scope）

用于设置单实例还是多实例

###### <2>、scope属性值

+ 默认值，singleton，表示是单实例对象
+  prototype，表示是多实例对象

![image-20220707185059239](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707185059239.png)

###### <3>、singleton和prototype区别

+ singleton单实例，prototype多实例
+ 设置scope值是singleton时候，加载spring配置文件时候就会创建单实例对象
+ 设置scope值是prototype时候，不是在加载spring配置文件时候创建 对象，在调用getBean方法时候创建多实例对象

#### (11)、生命周期

##### [1]、概念

从对象创建到对象销毁的过程

##### [2]、bean生命周期

###### <1>、通过构造器创建bean实例（无参数构造）

###### <2>、为bean的属性设置值和对其他bean引用（调用set方法）

###### <3>、调用bean的初始化的方法（需要进行配置初始化的方法）

###### <4>、bean可以使用了（对象获取到了）

###### <5>、当容器关闭时候，调用bean的销毁的方法（需要进行配置销毁的方法）

##### [3]、演示bean生命周期

###### <1>、类

```java
public class Orders {
//无参数构造 
    public Orders() { 
        System.out.println("第一步 执行无参数构造创建bean实例"); 
    } 
    private String oname; 
    public void setOname(String oname) { 
        this.oname = oname; 
        System.out.println("第二步 调用set方法设置属性值"); 
    } 
    //创建执行的初始化的方法 
    public void initMethod() { 
        System.out.println("第三步 执行初始化的方法");
    } 
    //创建执行的销毁的方法 
    public void destroyMethod() { 
        System.out.println("第五步 执行销毁的方法"); 
    } 
}
```

###### <2>、配置

```xml
<bean id="orders" class="com.atguigu.spring5.bean.Orders" init-method="initMethod" destroy-method="destroyMethod"> 
    <property name="oname" value="手机"></property> 
</bean>
```

###### <3>、测试

```java
@Test 
public void testBean3() { 
    // ApplicationContext context = // new ClassPathXmlApplicationContext("bean4.xml"); 
    ClassPathXmlApplicationContext context = new 
        ClassPathXmlApplicationContext("bean4.xml"); 
    Orders orders = context.getBean("orders", Orders.class); 
    System.out.println("第四步 获取创建bean实例对象"); 
    System.out.println(orders); 
    //手动让bean实例销毁 
    context.close(); 
}
```

###### <4>、结果

![image-20220707190008739](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707190008739.png)

##### [4]、加入后置处理器的生命周期

###### <1>、通过构造器创建bean实例（无参数构造）

###### <2>、为bean的属性设置值和对其他bean引用（调用set方法） 

###### <3>、把bean实例传递bean后置处理器的方法postProcessBeforeInitialization

###### <4>、调用bean的初始化的方法（需要进行配置初始化的方法）

###### <5>、把bean实例传递bean后置处理器的方法 postProcessAfterInitialization

###### <6>、bean可以使用了（对象获取到了）

###### <7>、当容器关闭时候，调用bean的销毁的方法（需要进行配置销毁的方法）

##### [5]、演示添加后置处理器效果

###### <1>、创建类，实现接口BeanPostProcessor，创建后置处理器

```java
public class MyBeanPost implements BeanPostProcessor {
    @Override 
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException { 
        System.out.println("在初始化之前执行的方法"); 
        return bean; 
    } 
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException { 
        System.out.println("在初始化之后执行的方法"); 
        return bean; 
    } 
}
```

<2>、配置

```xml
<!--配置后置处理器--> 
<bean id="myBeanPost" class="com.atguigu.spring5.bean.MyBeanPost"></bean>
```

###### <3>、结果

![image-20220707190626580](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707190626580.png)

#### (12)、自动装配

##### [1]、什么是自动装配

根据指定装配规则（属性名称或者属性类型），Spring自动将匹配的属性值进行注入

##### [2]、演示自动装配过程

###### <1>、根据属性名称自动注入

```xml
 <!--
实现自动装配 
bean标签属性autowire，配置自动装配 autowire属性常用两个值： 
byName根据属性名称注入 ，注入值bean的id值和类属性名称一样 
byType根据属性类型注入
 --> 
<bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byName"> 
    <!--<property name="dept" ref="dept"></property>-->
</bean>

<bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```

###### <2>、根据属性类型自动注入 

```xml
<!--
实现自动装配 
bean标签属性autowire，配置自动装配 autowire属性常用两个值： 
byName根据属性名称注入 ，注入值bean的id值和类属性名称一样 
byType根据属性类型注入 
--> 
<bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byType"> 
    <!--<property name="dept" ref="dept"></property>-->
</bean> 

<bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```

#### (13)、外部属性文件

##### [1]、直接配置数据库信息

###### <1>、配置德鲁伊连接池

```xml
<!--直接配置连接池--> 
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"> 
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property> 
    <property name="url" value="jdbc:mysql://localhost:3306/userDb"></property> 
    <property name="username" value="root"></property> 
    <property name="password" value="root"></property> 
</bean>
```

###### <2>、引入德鲁伊连接池依赖jar包

![image-20220707191337543](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707191337543.png)

##### [2]、外部属性文件配置数据库连接池

###### <1>、创建外部属性文件，properties格式文件，写数据库信息

![image-20220707191437620](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707191437620.png)

###### <2>、把外部properties属性文件引入到spring配置文件中

* 引入context名称空间 

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p" xmlns:util="http://www.springframework.org/schema/util" xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
  </beans>
  ```

+ 在spring配置文件使用标签引入外部属性文件 

  ```xml
  <!--引入外部属性文件--> 
  <context:property-placeholder location="classpath:jdbc.properties"/> <!--配置连接池--> 
  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"> 
      <property name="driverClassName" value="${prop.driverClass}"></property> 
      <property name="url" value="${prop.url}"></property> 
      <property name="username" value="${prop.userName}"></property> 
      <property name="password" value="${prop.password}"></property> 
  </bean>
  ```

### c、基于注解

#### (1)、注解概念

+ 注解是代码特殊标记，格式：@注解名称(属性名称=属性值, 属性名称=属性值..)
+ 使用注解，注解作用在类上面，方法上面，属性上面
+ 使用注解目的：简化xml配置

#### (2)、Spring提供的注解

+ @Component
+ @Service
+ @Controller
+ @Repository

> 上面四个注解功能是一样的，都可以用来创建bean实例

#### (3)、对象创建

##### [1] 、引入依赖

##### [2]、开启组件扫描 

```xml
<!--开启组件扫描 1 如果扫描多个包，多个包使用逗号隔开 2 扫描包上层目录 --> <context:component-scan base-package="com.atguigu"></context:component-scan>
```

##### [3]、 创建类，在类上面添加创建对象注解 

```java
//在注解里面value属性值可以省略不写， //默认值是类名称，首字母小写 //UserService -- userService
@Component(value = "userService") 
//<bean id="userService" class=".."/> 
public class UserService { 
    public void add() { 
        System.out.println("service add......."); 
    } 
}
```

#### (4)、开启组件扫描细节配置 

```xml
<!--示例1 use-default-filters="false" 表示现在不使用默认filter，自己配置filter context:include-filter ，设置扫描哪些内容 --> 
<context:component-scan base-package="com.atguigu" use-default-filters="false"> 
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/> 
</context:component-scan> 


<!--示例2 下面配置扫描包所有内容 
context:exclude-filter： 设置哪些内容不进行扫描 --> 
<context:component-scan base-package="com.atguigu"> 
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/> 
</context:component-scan>
```

#### (5)、属性注入

##### [1]、@Autowired：根据属性类型进行自动装配

###### <1>、把service和dao对象创建，在service和dao类添加创建对象注解

###### <2>、在service注入dao对象，在service类添加dao类型属性，在属性上面使用注解 

```java
@Service 
public class UserService { 
    //定义dao类型属性 
    //不需要添加set方法 
    //添加注入属性注解 
    @Autowired
    private UserDao userDao; 
    public void add() { 
        System.out.println("service add......."); 
        userDao.add(); 
    } 
}
```

##### [2]、@Qualifier：根据名称进行注入

这个@Qualifier注解的使用，和上面@Autowired一起使用 

```java
//定义dao类型属性 
//不需要添加set方法
//添加注入属性注解
@Autowired //根据类型进行注入 
@Qualifier(value = "userDaoImpl1") //根据名称进行注入 
private UserDao userDao;
```

##### [3]、@Resource：可以根据类型注入，可以根据名称注入

```java
 //@Resource //根据类型进行注入 
@Resource(name = "userDaoImpl1") //根据名称进行注入 
private UserDao userDao;
```

##### [4]、@Value：注入普通类型属性 

```java
@Value(value = "abc") 
private String name;
```

#### (6)、完全注解开发

##### [1]、创建配置类，替代xml配置文件

```java
@Configuration 
//作为配置类，替代xml配置文件 
@ComponentScan(basePackages = {"com.atguigu"}) 
public class SpringConfig { } 
```

##### [2]、编写测试类 

```java
@Test 
public void testService2() { 
    //加载配置类 
    ApplicationContext context = new 
        AnnotationConfigApplicationContext(SpringConfig.class); 
    UserService userService = context.getBean("userService", 
                                              UserService.class); 
    System.out.println(userService); 
    userService.add(); 
}
```



# 三、Aop 

## 1、概念

### a、什么是AOP 

+ 面向切面编程（方面），利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
+ 通俗描述：不通过修改源代码方式，在主干功能里面添加新功能
+ 使用登录例子说明AOP
+ ![image-20220707194604267](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707194604267.png)

## 2、底层原理

AOP底层使用动态代理

### a、JDK动态代理

有接口，创建接口实现类代理对象，增强类的方法

![image-20220707194811993](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707194811993.png)

#### (1)、实现思路

##### [1]、使用Proxy类里面的方法创建代理对象

![image-20220707195025883](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707195025883.png)

##### [2]、调用newProxyInstance方法

方法有三个参数：

+ 类加载器
+ 增强方法所在的类，这个类实现的接口，支持多个接口
+ 实现这个接口InvocationHandler，创建代理对象，写增强的部分

#### (2)、编写代码

##### [1]创建接口，定义方法 

```java
public interface UserDao { 
    public int add(int a,int b); 
    public String update(String id);
}
```

##### [2]、创建接口实现类，实现方法 

```java
public class UserDaoImpl implements UserDao { 
@Override 
    public int add(int a, int b) { 
        return a+b; 
    } 
    @Override 
    public String update(String id) { 
        return id; 
    } 
}
```

##### [3]、使用Proxy类创建接口代理对象 

```java
public class JDKProxy { 
    public static void main(String[] args) { 
        //创建接口实现类代理对象 
        Class[] interfaces = {UserDao.class};  
        UserDaoImpl userDao = new UserDaoImpl(); 
        UserDao dao = 
            (UserDao)Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), 
                                            interfaces, 
                                            new UserDaoProxy(userDao));
        int result = dao.add(1, 2); System.out.println("result:"+result); 
    } 
} 


//创建代理对象代码 
class UserDaoProxy implements InvocationHandler { 
    //1 把创建的是谁的代理对象，把谁传递过来 
    //有参数构造传递 
    private Object obj; 
    public UserDaoProxy(Object obj) { this.obj = obj; } 
    //增强的逻辑
    @Override 
    public Object invoke(Object proxy, Method method, Object[] args) throws 
        Throwable { 
        //方法之前 
        System.out.println("方法之前执行...."+method.getName()+" :传递的参数..."+ 
                           Arrays.toString(args)); 
        //被增强的方法执行 
        Object res = method.invoke(obj, args); 
        //方法之后
        System.out.println("方法之后执行...."+obj);
        return res;
    } 
}
```

### b、CGLIB动态代理

无接口，创建子类的代理对象，增强类的方法

![image-20220707194836249](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220707194836249.png)

## 3、术语

### a、连接点

可以被增强的方法

### b、切入点

实际被增强的方法

### c、通知

实际被增强的部分

通知的分类：

+ 前置通知
+ 后置通知
+ 环绕通知
+ 异常通知
+ 最终通知

### d、切面

把通知应用到切入点的过程

## 4、操作

### a、准备工作

#### (1)、基于AspectJ实现AOP操作

AspectJ不是Spring组成部分，独立AOP框架，一般把AspectJ和Spirng框架一起使用，进行AOP操作

#### (2)、基于AspectJ实现AOP操作

##### [1]、基于xml配置文件实现

##### [2]、基于注解方式实现（使用）

#### (3)、在项目工程里面引入AOP相关依赖

#### (4)、切入点表达式

##### [1]、切入点表达式作用

知道对哪个类里面的哪个方法进行增强

##### [2]、语法结构

 execution([权限修饰符] [返回类型] [类全路径] [方法名称]([参数列表]) )

+ 举例1：对com.atguigu.dao.BookDao类里面的add进行增强
  execution(* com.atguigu.dao.BookDao.add(..))
+ 举例2：对com.atguigu.dao.BookDao类里面的所有的方法进行增强
  execution(* com.atguigu.dao.BookDao.* (..))
+ 举例3：对com.atguigu.dao包里面所有类，类里面所有方法进行增强
  execution(* com.atguigu.dao.*.* (..))

### b、AspectJ 注解操作

#### (1)、创建类，在类里面定义方法 

```java
public class User { 
    public void add() { 
        System.out.println("add......."); 
    } 
}
```

#### (2)、创建增强类（编写增强逻辑）

在增强类里面，创建方法，让不同方法代表不同通知类型 \

```java
//增强的类 
public class UserProxy { 
    public void before() {
        //前置通知 
        System.out.println("before......");
    } 
}
```

#### (3)、进行通知的配置

##### [1]、在spring配置文件中，开启注解扫描 

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"> 
    <!-- 开启注解扫描 --> 
    <context:component-scan base-package="com.atguigu.spring5.aopanno">
    </context:component-scan>
</beans>
```

##### [2]、使用注解创建User和UserProxy对象

![image-20220708092937622](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708092937622.png)

![image-20220708092949168](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708092949168.png)

##### [3]、在增强类上面添加注解 @Aspect 

```java
//增强的类 
@Component 
@Aspect 
//生成代理对象 
public class UserProxy {}
```

##### [4]、在spring配置文件中开启生成代理对象 

```xml
<!-- 开启Aspect生成代理对象--> 
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

#### (4)、配置不同类型的通知

在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置

```java
 //增强的类 
@Component 
@Aspect 
//生成代理对象 
public class UserProxy { 
    //前置通知 
    //@Before注解表示作为前置通知 
    @Before(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))") 
    public void before() { 
        System.out.println("before.........");
    } 
    //后置通知（返回通知） 
    @AfterReturning(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))") 
    public void afterReturning() { 
        System.out.println("afterReturning........."); 
    } 
    //最终通知
    @After(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))") 
    public void after() { 
        System.out.println("after........."); 
    } 
    //异常通知 
    @AfterThrowing(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))") 
    public void afterThrowing() {
        System.out.println("afterThrowing.........");
    } 
    //环绕通知 
    @Around(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))") 
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable { 
        System.out.println("环绕之前........."); 
        //被增强的方法执行
        proceedingJoinPoint.proceed(); 
        System.out.println("环绕之后........."); 
    } 
}
```



#### (5)、相同的切入点抽取

```java
//相同切入点抽取 
@Pointcut(value = "execution(* com.atguigu.spring5.aopanno.User.add(..))")
public void pointdemo() { } 

//前置通知 
//@Before注解表示作为前置通知
@Before(value = "pointdemo()") 
public void before() { 
    System.out.println("before.........");
   }
```

#### (6)、有多个增强类多同一个方法进行增强，设置增强类优先级

在增强类上面添加注解 @Order(数字类型值)，数字类型值越小优先级越高 

```java
@Component 
@Aspect 
@Order(1) 
public class PersonProxy {}
```



#### (7)、完全使用注解开发 

创建配置类，不需要创建xml配置文件 

```java
@Configuration
@ComponentScan(basePackages = {"com.atguigu"}) @EnableAspectJAutoProxy(proxyTargetClass = true) 
public class ConfigAop { }
```

### c、AspectJ 配置文件操作

#### (1)、创建两个类，增强类和被增强类，创建方法

#### (2)、在spring配置文件中创建两个类对象 

```xml
<!--创建对象--> 
<bean id="book" class="com.atguigu.spring5.aopxml.Book"></bean> 
<bean id="bookProxy" class="com.atguigu.spring5.aopxml.BookProxy"></bean>
```

#### (3)、在spring配置文件中配置切入点 

```xml
<!--配置aop增强--> 
<aop:config> 
    <!--切入点--> 
    <aop:pointcut id="p" expression="execution(* 
                                     com.atguigu.spring5.aopxml.Book.buy(..))"/> 
    <!--配置切面--> 
    <aop:aspect ref="bookProxy"> 
        <!--增强作用在具体的方法上-->
        <aop:before method="before" pointcut-ref="p"/> 
    </aop:aspect> 
</aop:config>
```

# 四、JdbcTemplate

## 1、概念

Spring框架对JDBC进行封装，使用JdbcTemplate方便实现对数据库操作

## 2、准备工作

### a、引入相关jar包

![image-20220708094439050](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708094439050.png)

### b、在spring配置文件配置数据库连接池

```xml
<!-- 数据库连接池 --> 
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close"> 
    <property name="url" value="jdbc:mysql:///user_db" /> 
    <property name="username" value="root" /> 
    <property name="password" value="root" /> 
    <property name="driverClassName" value="com.mysql.jdbc.Driver" /> 
</bean>
```

### c、配置JdbcTemplate对象，注入DataSource

```xml
<!-- JdbcTemplate对象 --> 
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate"> 
    <!--注入dataSource--> 
    <property name="dataSource" ref="dataSource"></property> 
</bean>
```

### d、创建service类，创建dao类，在dao注入jdbcTemplate对象

#### (1)、配置文件 

```xml
<!-- 组件扫描 -->
<context:component-scan base-package="com.atguigu"></context:component-scan>
```

#### (2)、Service 

```java
@Service 
public class BookService { 
    //注入dao 
    @Autowired 
    private BookDao bookDao; 
}
```

#### (3)、 Dao

```java
@Repository 
public class BookDaoImpl implements BookDao { 
    //注入JdbcTemplate
    @Autowired
    private JdbcTemplate jdbcTemplate;
}
```

## 3、操作

### a、添加

#### (1)、对应数据库创建实体类

![image-20220708103306413](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708103306413.png)

#### (2)、编写service和dao

##### [1]、在dao进行数据库添加操作

##### [2]、调用JdbcTemplate对象里面update方法实现添加操作

![image-20220708103439237](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708103439237.png)

> 第一个参数：sql语句
>  第二个参数：可变参数，设置sql语句值

```java
@Repository
public class BookDaoImpl implements BookDao { 
    //注入JdbcTemplate 
    @Autowired private JdbcTemplate jdbcTemplate; 
    //添加的方法 
    @Override public void add(Book book) { 
        //1 创建sql语句
        String sql = "insert into t_book values(?,?,?)"; 
        //2 调用方法实现
        Object[] args = {book.getUserId(), book.getUsername(), book.getUstatus()}; 
        int update = jdbcTemplate.update(sql,args);

        System.out.println(update); 
    } 
}
```

##### [3]、测试

```java
@Test 
public void testJdbcTemplate() { 
    ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml"); 
    BookService bookService = context.getBean("bookService", BookService.class); 
    Book book = new Book(); 
    book.setUserId("1");
    book.setUsername("java"); 
    book.setUstatus("a"); 
    bookService.addBook(book); 
}
```



### b、修改

```java
@Override 
public void updateBook(Book book) { 
    String sql = "update t_book set username=?,ustatus=? where user_id=?"; 
    Object[] args = {book.getUsername(), book.getUstatus(),book.getUserId()}; 
    int update = jdbcTemplate.update(sql, args); 
    System.out.println(update); 
}
```

### c、删除

```java
@Override 
public void delete(String id) { 
    String sql = "delete from t_book where user_id=?"; 
    int update = jdbcTemplate.update(sql, id); System.out.println(update); 
}
```

### d、查询

#### (1)、查询返回某个值

![image-20220708103839487](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708103839487.png)

> 第一个参数：sql语句
> 第二个参数：返回类型Class

```java
//查询表记录数 
@Override
public int selectCount() {
    String sql = "select count(*) from t_book"; 
    Integer count = jdbcTemplate.queryForObject(sql, Integer.class); return count; 
}
```

#### (2)、查询返回对象

![image-20220708104007303](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708104007303.png)

> 第一个参数：sql语句
> 第二个参数：RowMapper是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装
> 第三个参数：sql语句值

```java
//查询返回对象 
@Override 
public Book findBookInfo(String id) { 
    String sql = "select * from t_book where user_id=?"; 
    //调用方法 
    Book book = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<Book>
                                            (Book.class), id); 
    return book; 
}
```

#### (3)、查询返回集合

![image-20220708104142856](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708104142856.png)

> 第一个参数：sql语句
>  第二个参数：RowMapper是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装
>  第三个参数：sql语句值

```java
//查询返回集合 
@Override public List<Book> findAllBook() {
    String sql = "select * from t_book"; 
    //调用方法 
    List<Book> bookList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<Book>
                                             (Book.class)); 
    return bookList; 
}
```

### e、批量操作

![image-20220708104324089](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708104324089.png)

> 第一个参数：sql语句
>  第二个参数：List集合，添加多条记录数据

#### (1)、批量添加

```java
//批量添加 
@Override 
public void batchAddBook(List<Object[]> batchArgs) {
    String sql = "insert into t_book values(?,?,?)"; 
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs); 
    System.out.println(Arrays.toString(ints)); 
} 

//批量添加测试 
List<Object[]> batchArgs = new ArrayList<>();
Object[] o1 = {"3","java","a"}; 
Object[] o2 = {"4","c++","b"}; 
Object[] o3 = {"5","MySQL","c"}; 
batchArgs.add(o1); 
batchArgs.add(o2); 
batchArgs.add(o3); 
//调用批量添加 
bookService.batchAdd(batchArgs);
```

#### (2)、批量修改

```java
//批量修改
@Override 
public void batchUpdateBook(List<Object[]> batchArgs) { 
    String sql = "update t_book set username=?,ustatus=? where user_id=?"; 
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs); 
    System.out.println(Arrays.toString(ints)); 
} 
//批量修改
List<Object[]> batchArgs = new ArrayList<>(); 
Object[] o1 = {"java0909","a3","3"}; 
Object[] o2 = {"c++1010","b4","4"}; 
Object[] o3 = {"MySQL1111","c5","5"};
batchArgs.add(o1); 
batchArgs.add(o2); 
batchArgs.add(o3);
//调用方法实现批量修改 
bookService.batchUpdate(batchArgs);
```

#### (3)、批量删除

```java
//批量删除
@Override 
public void batchDeleteBook(List<Object[]> batchArgs) { 
    String sql = "delete from t_book where user_id=?"; 
    int[] ints = jdbcTemplate.batchUpdate(sql, batchArgs); 
    System.out.println(Arrays.toString(ints)); } 
//批量删除
List<Object[]> batchArgs = new ArrayList<>();
Object[] o1 = {"3"}; 
Object[] o2 = {"4"}; 
batchArgs.add(o1);
batchArgs.add(o2); 
//调用方法实现批量删除 
bookService.batchDelete(batchArgs);
```

# 五、事务

## 1、概念

### a、什么事务

事务是数据库操作最基本单元，逻辑上一组操作，要么都成功，如果有一个失败所有操作都失败
典型场景：银行转账

* lucy 转账100元 给mary
* lucy少100，mary多100

### b、事务四个特性（ACID）

+ 原子性
+ 一致性
+ 隔离性
+ 持久性

## 2、环境搭建

![image-20220708105112849](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708105112849.png)

### a、创建数据库表，添加记录

![image-20220708105142683](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708105142683.png)

### b、创建service，搭建dao，完成对象创建和注入关系

service注入dao，在dao注入JdbcTemplate，在JdbcTemplate注入DataSource

```java
@Service 
public class UserService { 
    //注入dao
    @Autowired 
    private UserDao userDao;
} 
@Repository 
public class UserDaoImpl implements UserDao { 
    @Autowired
    private JdbcTemplate jdbcTemplate; 
}
```

### c、创建方法

在dao创建两个方法：多钱和少钱的方法，在service创建方法（转账的方法）

```java
@Repository
public class UserDaoImpl implements UserDao { 
    @Autowired 
    private JdbcTemplate jdbcTemplate; 
    //lucy转账100给mary 
    //少钱 
    @Override 
    public void reduceMoney() { 
        String sql = "update t_account set money=money-? where username=?"; 
        jdbcTemplate.update(sql,100,"lucy"); } 
    //多钱 
    @Override public void addMoney() {
        String sql = "update t_account set money=money+? where username=?"; 
        jdbcTemplate.update(sql,100,"mary"); 
    } 
}
@Service
public class UserService {
    //注入dao 
    @Autowired
    private UserDao userDao; 
    //转账的方法 
    public void accountMoney() { 
        //lucy少100 
        userDao.reduceMoney();
        //mary多100 
        userDao.addMoney(); 
    } 
}
```

### d、模拟异常

上面代码，如果正常执行没有问题的，但是如果代码执行过程中出现异常，有问题

![image-20220708105559297](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708105559297.png)

### e、事务操作

![image-20220708105631270](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708105631270.png)

## 3、事务管理

> 事务添加到JavaEE三层结构里面Service层（业务逻辑层）

在Spring进行事务管理操作有两种方式：

+ 编程式事务管理
+ 声明式事务管理（使用）

声明式事务管理

+ 基于注解方式（使用）
+ 基于xml配置文件方式
  

> 在Spring进行声明式事务管理，底层使用AOP原理

Spring事务管理API，提供一个接口，代表事务管理器，这个接口针对不同的框架提供不同的实现类

![image-20220708110006470](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708110006470.png)

## 4、事务操作注解声明

### a、在spring配置文件配置事务管理器 

```xml
<!--创建事务管理器--> 
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"> 
    <!--注入数据源--> 
    <property name="dataSource" ref="dataSource"></property> 
</bean>
```

### b、在spring配置文件，开启事务注解

#### (1)、在spring配置文件引入名称空间 tx 

```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context" xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
</beans>
```

#### (2)、开启事务注解 

```xml
<!--开启事务注解--> 
<tx:annotation-driven transaction-manager="transactionManager">
</tx:annotation-driven>
```

### c、在service类上面（或者service类里面方法上面）添加事务注解

@Transactional，这个注解添加到类上面，也可以添加方法上面

> + 如果把这个注解添加类上面，这个类里面所有的方法都添加事务
> + 如果把这个注解添加方法上面，为这个方法添加事务 @Service @Transactional public class UserService {

### d、参数配置器

在service类上面添加注解@Transactional，在这个注解里面可以配置事务相关参数

![image-20220708110554680](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708110554680.png)

#### (1)、propagation：事务传播行为

多事务方法直接进行调用，这个过程中事务 是如何进行管理的

![image-20220708110713661](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708110713661.png)

![image-20220708110725934](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708110725934.png)

![image-20220708110741278](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708110741278.png)

#### (2)、ioslation：事务隔离级别

事务有特性成为隔离性，多事务操作之间不会产生影响。不考虑隔离性产生很多问题

> 有三个读问题：脏读、不可重复读、虚（幻）读

##### [1]、脏读

一个未提交事务读取到另一个未提交事务的数据

![image-20220708110856905](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708110856905.png)

##### [2]、不可重复读

一个未提交事务读取到另一提交事务修改数据

![image-20220708110947345](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708110947345.png)

##### [3]、虚读

一个未提交事务读取到另一提交事务添加数据

##### [4]、解决方案

设置隔离级别

![image-20220708111126329](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708111126329.png)

![image-20220708111152043](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708111152043.png)

#### (3)、timeout：超时时间

事务需要在一定时间内进行提交，如果不提交进行回滚

> 默认值是 -1 ，设置时间以秒单位进行计算

#### (4)、readOnly：是否只读

读：查询操作，写：添加修改删除操作
readOnly默认值false，表示可以查询，可以添加修改删除操作

> 设置readOnly值是true，设置成true之后，只能查询

#### (5)、rollbackFor：回滚

设置出现哪些异常进行事务回滚

#### (6)、noRollbackFor：不回滚

设置出现哪些异常不进行事务回滚

## 5、事务操作XML声明

### a、在spring配置文件中进行配置

#### (1)、 配置事务管理器

```xml
<!--1 创建事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager"> 
    <!--注入数据源--> 
    <property name="dataSource" ref="dataSource"></property> 
</bean> 
```

#### (2) 、配置通知

```xml
<!--2 配置通知-->
<tx:advice id="txadvice">
    <!--配置事务参数--> 
    <tx:attributes> 
        <!--指定哪种规则的方法上面添加事务-->
        <tx:method name="accountMoney" propagation="REQUIRED"/>
        <!--<tx:method name="account*"/>--> 
    </tx:attributes> 
</tx:advice> 
```

#### (3)、配置切入点和切面 

```xml
<!--3 配置切入点和切面--> 
<aop:config> <!--配置切入点--> 
    <aop:pointcut id="pt" expression="execution(* 
                                      com.atguigu.spring5.service.UserService.*
                                      (..))"/> 
    <!--配置切面--> 
    <aop:advisor advice-ref="txadvice" pointcut-ref="pt"/> 
</aop:config>
```

## 6、事务操作完全注解声明

创建配置类，使用配置类替代xml配置文件

```java
 @Configuration 
//配置类 
@ComponentScan(basePackages = "com.atguigu") 
//组件扫描
@EnableTransactionManagement 
//开启事务 
public class TxConfig { 
    //创建数据库连接池 
    @Bean public DruidDataSource getDruidDataSource() {
        DruidDataSource dataSource = new DruidDataSource(); 
        dataSource.setDriverClassName("com.mysql.jdbc.Driver"); 
        dataSource.setUrl("jdbc:mysql:///user_db"); dataSource.setUsername("root"); 
        dataSource.setPassword("root"); 
        return dataSource; 
    }
    //创建JdbcTemplate对象
    @Bean 
    public JdbcTemplate getJdbcTemplate(DataSource dataSource) { 
    //到ioc容器中根据类型找到dataSource 
        JdbcTemplate jdbcTemplate = new JdbcTemplate(); 
        //注入dataSource 
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    } 
    //创建事务管理器
    @Bean public DataSourceTransactionManager
        getDataSourceTransactionManager(DataSource dataSource) { 
        DataSourceTransactionManager transactionManager = new 
            DataSourceTransactionManager(); 
        transactionManager.setDataSource(dataSource); 
        return transactionManager;
    } 
}
```

# 六、Spring5 新特性

## 1、基于jdk8

整个Spring5框架的代码基于Java8，运行时兼容JDK9，许多不建议使用的类和方法在代码库中删除

## 2、自带日志 

Spring5已经移除Log4jConfigListener，官方建议使用Log4j2 

### a、Spring5框架整合Log4j2 

#### (1)、 引入jar包

![image-20220708112820378](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708112820378.png)

#### (2)、创建log4j2.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL --> <!--Configuration后面的status用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，可以看到log4j2内部各种详细输出--> 
<configuration status="INFO"> 
    <!--先定义所有的appender--> 
    <appenders> 
        <!--输出日志信息到控制台--> 
        <console name="Console" target="SYSTEM_OUT">
            <!--控制日志输出的格式--> 
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level 
                                    %logger{36} - %msg%n"/> 
        </console> 
    </appenders> 
    <!--然后定义logger，只有定义logger并引入的appender，appender才会生效-->
    <!--root：用于指定项目的根日志，如果没有单独指定Logger，则会使用root作为默认的日志输出--> 
    <loggers> 
        <root level="info"> <
            appender-ref ref="Console"/> 
        </root>
    </loggers> 
</configuration>
```

## 3、@Nullable注解 

@Nullable注解可以使用在方法上面，属性上面，参数上面，表示方法返回可以为空，属性值可以为空，参数值可以为空

+ 注解用在方法上面，方法返回值可以为空 
+ 注解使用在方法参数里面，方法参数可以为空
+ 注解使用在属性上面，属性值可以为空

## 4、支持函数式风格

GenericApplicationContext

```java
//函数式风格创建对象，交给spring进行管理 
@Test public void testGenericApplicationContext() { 
    //1 创建GenericApplicationContext对象 
    GenericApplicationContext context = new GenericApplicationContext(); 
    //2 调用context的方法对象注册 
    context.refresh(); 
    context.registerBean("user1",User.class,() -> new User()); 
    //3 获取在spring注册的对象 
    // User user = (User)context.getBean("com.atguigu.spring5.test.User");
    User user = (User)context.getBean("user1");
    System.out.println(user); 
}
```

## 5、整合JUnit5

### a、整合JUnit4

#### (1)、 引入依赖

![image-20220708113509364](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708113509364.png)

#### (2)、 创建测试类，使用注解方式

```java
@RunWith(SpringJUnit4ClassRunner.class) //单元测试框架 @ContextConfiguration("classpath:bean1.xml") //加载配置文件
public class JTest4 {
    @Autowired 
    private UserService userService; 
    @Test 
    public void test1() {
        userService.accountMoney();
    }
}
```

### b、Spring5整合JUnit5

#### (1)、引入JUnit5的jar包

![image-20220708113656462](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708113656462.png)

#### (2)、创建测试类，使用注解

```java
@ExtendWith(SpringExtension.class) 
@ContextConfiguration("classpath:bean1.xml") 
public class JTest5 { 
    @Autowired
    private UserService userService;
    @Test public void test1() { 
        userService.accountMoney();
    } 
}
```

#### (3)、复合注解

替代上面两个注解完成整合 

```java
@SpringJUnitConfig(locations = "classpath:bean1.xml") 
public class JTest5 { 
    @Autowired 
    private UserService userService;
    @Test 
    public void test1() { 
        userService.accountMoney(); 
    } 
}
```

## 6、Webflux

### a、SpringWebflux介绍

#### (1)、web框架

是Spring5添加新的模块，用于web开发的，功能和SpringMVC类似的，Webflux使用当前一种比较流程响应式编程出现的框架。

![image-20220708114449723](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708114449723.png)

#### (2)、框架特性

使用传统web框架，比如SpringMVC，这些基于Servlet容器，Webflux是一种异步非阻塞的框架，异步非阻塞的框架在Servlet3.1以后才支持，核心是基于Reactor的相关API实现的。

#### (3)、异步非阻塞

* 异步和同步

  > 异步和同步针对调用者，调用者发送请求，如果等着对方回应之后才去做其他事情就是同步，
  >
  > 如果发送请求之后不等着对方回应就去做其他事情就是异步

* 非阻塞和阻塞

> 阻塞和非阻塞针对被调用者，被调用者受到请求之后，做完请求任务之后才给出反馈就是阻塞，
>
> 受到请求之后马上给出反馈然后再去做事情就是非阻塞

#### (4)、Webflux特点：

+ 非阻塞式：在有限资源下，提高系统吞吐量和伸缩性，以Reactor为基础实现响应式编程
+ 函数式编程：Spring5框架基于java8，Webflux使用Java8函数式编程方式实现路由请求

#### (5)、比较SpringMVC

![image-20220708114929156](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708114929156.png)

> SpringMVC采用命令式编程，Webflux采用异步响应式编程 
>
>  两个框架都可以使用注解方式，都运行在Tomet等容器中

### b、响应式编程(Java实现)

#### (1)、什么是响应式编程 

响应式编程是一种面向数据流和变化传播的编程范式。

这意味着可以在编程语言中很方便地表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播。 

电子表格程序就是响应式编程的一个例子。

单元格可以包含字面值或类似"=B1+C1"的公式，而包含公式的单元格的值会依据其他单元格的值的变化而变化。

#### (2)、Java8及其之前版本

提供的观察者模式两个类Observer和Observable 

```java
public class ObserverDemo extends Observable { 
    public static void main(String[] args) {
        ObserverDemo observer = new ObserverDemo(); 
        //添加观察者 
        observer.addObserver((o,arg)->{ System.out.println("发生变化"); }); 
        observer.addObserver((o,arg)->{ System.out.println("手动被观察者通知，准备改变"); }); 
        observer.setChanged(); 
        //数据变化
        observer.notifyObservers(); 
        //通知 
    } 
}
```

### c、响应式编程（Reactor实现）

响应式编程操作中，Reactor是满足Reactive规范框架



Reactor有两个核心类，Mono和Flux，这两个类实现接口Publisher，提供丰富操作符。

+ Flux对象实现发布者，返回N个元素；
+ Mono实现发布者，返回0或者1个元素 



Flux和Mono都是数据流的发布者，使用Flux和Mono都可以发出三种数据信号： 元素值，错误信号，完成信号

> 错误信号和完成信号都代表终止信号，终止信号用于告诉订阅者数据流结束了，错误信号终止数据流同时把错误信息传递给订阅者

![](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708115503927.png)

#### (1)、代码演示 Flux和Mono

##### [1]、 引入依赖 

```xml
<dependency> 
    <groupId>io.projectreactor</groupId> 
    <artifactId>reactor-core</artifactId> 
    <version>3.1.5.RELEASE</version> 
</dependency>
```

##### [2]、编程代码 

```java
public static void main(String[] args) { 
    //just方法直接声明 
    Flux.just(1,2,3,4); 
    Mono.just(1); 
    //其他的方法 
    Integer[] array = {1,2,3,4}; 
    Flux.fromArray(array); 
    List<Integer> list = Arrays.asList(array); 
    Flux.fromIterable(list);
    Stream<Integer> stream = list.stream(); 
    Flux.fromStream(stream); 
}
```

#### (2)、三种信号特点

* 错误信号和完成信号都是终止信号，不能共存的
* 如果没有发送任何元素值，而是直接发送错误或者完成信号，表示是空数据流
* 如果没有错误信号，没有完成信号，表示是无限数据流

#### (3)、声明数据流

调用just或者其他方法只是声明数据流，数据流并没有发出，只有进行订阅之后才会触发数据流，不订阅什么都不会发生的

![image-20220708115944168](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708115944168.png)

#### (4)、操作符

对数据流进行一道道操作，成为操作符，比如工厂流水线

##### [1]、map 元素映射为新元素

![image-20220708120110012](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708120110012.png)

##### [2]、latMap 元素映射为流

 把每个元素转换流，把转换之后多个流合并大的流

![image-20220708120142009](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708120142009.png)

### d、核心API

SpringWebflux基于Reactor，默认使用容器是Netty，Netty是高性能的NIO框架，异步非阻塞的框架

#### (1)、Netty

##### [1]、BIO![image-20220708120320033](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708120320033.png)

[2]、NIO

![image-20220708120336652](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708120336652.png)

#### (2)、执行过程

SpringWebflux执行过程和SpringMVC相似的

* SpringWebflux核心控制器 DispatchHandler，实现接口WebHandler
* 接口WebHandler有一个方法

![](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708120418770.png)

#### ![image-20220708120456354](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708120456354.png)(3)、DispatcherHandler，负责请求的处理

* HandlerMapping：请求查询到处理的方法
* HandlerAdapter：真正负责请求处理
* HandlerResultHandler：响应结果处理

#### (4)、函数式编程

两个接口：

RouterFunction（路由处理）
HandlerFunction（处理函数）

### e、基于注解编程模型

SpringWebflux实现方式有两种：注解编程模型和函数式编程模型



使用注解编程模型方式，和之前SpringMVC使用相似的，只需要把相关依赖配置到项目中，SpringBoot自动配置相关运行容器，默认情况下使用Netty服务器

### f、基于函数式编程模型

在使用函数式编程模型操作时候，需要自己初始化服务器



基于函数式编程模型时候，有两个核心接口：RouterFunction（实现路由功能，请求转发给对应的handler）和HandlerFunction（处理请求生成响应的函数）。



核心任务定义两个函数式接口的实现并且启动需要的服务器。



SpringWebflux请求和响应不再是ServletRequest和ServletResponse，而是ServerRequest和ServerResponse