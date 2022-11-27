# 一、基于xml搭建

## 1、开发环境

+ IDE：idea 2022.1
+ 构建工具：maven 3.8.6
+ 服务器：tomcat 8
+ jdk：1.8
+ Spring版本：5.3.20

## 2、新建maven工程

### a、修改pom.xml

修改打包方式为war

![image-20220708163944877](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708163944877.png)

### b、修改maven版本

+ 进入maven的设置修改成自己安装的maven和本地仓库

![image-20220708163905412](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708163905412.png)

+ 修改自动更新

![image-20220708163837539](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708163837539.png)

## 3、添加web模块

+ 进入项目结构，新增web模块

![image-20220708164137612](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708164137612.png)

+ 修改web.xml的位置

![image-20220708164241019](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708164241019.png)

+ 修改web资源根目录

![image-20220708164310584](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708164310584.png)

## 4、添加依赖

#### (1)、SpringMVC、Spring

spring-webmvc

#### (2)、Spring-Jdbc

spring-jdbc

#### (3)、Spring面向切面编程：

spring-aspects

#### (4)、Mybatis

mybatis

#### (5)、mybatis整合Spring

mybatis-spring

#### (6)、数据库连接池

c3p0(不要用druid)

#### (7)、MySQL驱动

mysql-connector-java

#### (8)、其他（jstl，servlet-api，junit）

+ jstl
+ servlet-api 加上 <scope>provided</scope>
+ junit
+ slf4j

> 注意spring包的版本都要相同

pom.xml

```xml
<dependencies>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.20</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.3.20</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>5.3.20</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.7</version>
        </dependency>

        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.29</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/jstl/jstl -->
        <dependency>
            <groupId>jstl</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    
    	<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
		<dependency>
   			<groupId>org.slf4j</groupId>
   			<artifactId>slf4j-api</artifactId>
    		<version>1.7.30</version>
		</dependency>
     <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-nop -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.36</version>
        </dependency>

    </dependencies>
```

## 5、部署tomcat

+ 部署war包

![image-20220708170045249](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708170045249.png)

+ 修改服务器属性

![image-20220708170159034](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220708170159034.png)

## 6、整合spring-springmvc

### a、配置web.xml

#### (1)、启动Spring容器

启动Spring容器，设置 contextConfigLocation 和 ContextLoaderListener，contextConfigLocation设置spring配置文件的位置，在resources目录下创建applicationContext.xml文件

#### (2)、配置springMVC前端控制器

配置springMVC前端控制器dispatchServlet，拦截所有请求，需要指定springMVC配置文件的位置，在resources目录下创建springMVC.xml文件

> 若出现file is included in 4 contexts错误，是因为spring上下文环境过多，在项目结构的modle里，找到spring，去除所有上下文，再重新添加即可

#### (3)、配置字符编码过滤器

配置字符编码过滤器 CharacterEncodingFilter，初始化参数 encoding、forceRequestEncoding、forceResponseEncoding，设置过滤所有请求 /，一定要放在所有过滤器之前

#### (4)、配置Rest风格

使用Rest风格的URI，配置 HiddenHttpMethodFilter，将指定的 post 转化为 delete 或者是 put 请求。过滤所有请求/

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--1、启动Spring的容器  -->
    <!-- needed for ContextLoaderListener -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>

    <!-- Bootstraps the root web application context before servlet initialization -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!--2、springmvc的前端控制器，拦截所有请求  -->
    <!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
    <servlet>
        <servlet-name>springMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 通过初始化参数指定SpringMVC配置文件的位置和名称 -->
        <init-param>
            <!-- contextConfigLocation为固定值 -->
            <param-name>contextConfigLocation</param-name>
            <!-- 使用classpath:表示从类路径查找配置文件，例如maven工程中的src/main/resources -->
            <param-value>classpath:springMVC.xml</param-value>
        </init-param>
        <!--
             作为框架的核心组件，在启动过程中有大量的初始化操作要做
            而这些操作放在第一次请求时才执行会严重影响访问速度
            因此需要通过此标签将启动控制DispatcherServlet的初始化时间提前到服务器启动时
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMVC</servlet-name>
        <!--
            设置springMVC的核心控制器所能处理的请求的请求路径
            /所匹配的请求可以是/login或.html或.js或.css方式的请求路径
            但是/不能匹配.jsp请求路径的请求
        -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- 3、字符编码过滤器，一定要放在所有过滤器之前 -->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- 4、使用Rest风格的URI，将页面普通的post请求转为指定的delete或者put请求 -->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

### b、配置springMVC: springMVC.xml

#### (1)、自动扫描包

引入名称空间context，在Java目录下新建包以供扫描

#### (2)、配置视图解析器

修改pom.xml，添加thymeleaf依赖，配置thyme leaf视图解析器，对资源进行控制跳转等处理

pom.xml

```xml
 <!-- Spring5和Thymeleaf整合包 -->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
            <version>3.0.15.RELEASE</version>
        </dependency>
```

#### (3)、配置静态资源处理

引入名称空间mvc，启动Servlet默认处理器，处理springMVC不能处理的请求

#### (4)、配置注解驱动

springMVC.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 1、自动扫描包 -->
    <context:component-scan base-package="ningct.ssm.controller"/>

    <!-- 2、配置Thymeleaf视图解析器 -->
    <bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="order" value="1"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">

                        <!-- 视图前缀 -->
                        <property name="prefix" value="/WEB-INF/templates/"/>

                        <!-- 视图后缀 -->
                        <property name="suffix" value=".html"/>
                        <property name="templateMode" value="HTML5"/>
                        <property name="characterEncoding" value="UTF-8" />
                    </bean>
                </property>
            </bean>
        </property>
    </bean>

    <!--
       3、处理静态资源，例如html、js、css、jpg
      若只设置该标签，则只能访问静态资源，其他请求则无法访问
      此时必须设置<mvc:annotation-driven/>解决问题
     -->
    <mvc:default-servlet-handler/>

    <!-- 4、开启mvc注解驱动 -->
    <mvc:annotation-driven>
        <mvc:message-converters>
            <!-- 处理响应中文内容乱码 -->
            <bean class="org.springframework.http.converter.StringHttpMessageConverter">
                <property name="defaultCharset" value="UTF-8" />
                <property name="supportedMediaTypes">
                    <list>
                        <value>text/html</value>
                        <value>application/json</value>
                    </list>
                </property>
            </bean>
        </mvc:message-converters>
    </mvc:annotation-driven>
</beans>
```

### c、配置spring: applicationContext.xml

#### (1)、不扫描Controller

controller的扫描交给springmvc

#### (2)、配置数据源，创建jdbc.properties文件

记得在url加上连接的数据库

jdbc.properties

```properties
jdbc.url=jdbc:mysql://localhost:3306/ssm
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.user=ningct
jdbc.password=ning502502502
```

#### (3)、配置和mybatis的整合

##### [1]、创建sqlSessionFactory工厂对象

###### <1>、指定mybatis的核心配置文件

在resource下创建mybatis的核心配置文件mybatis-config.xml

###### <2>、指定数据源

###### <3>、指定mybatis的mapper文件

在resource目录下建立与Java目录下mapper同名的包，用来放置mapper映射文件

##### [2]、配置扫描器

将mybatis接口的实现加入到ioc容器中，扫描所有dao接口的实现，加入到ioc容器中

##### [3]、配置sqlSession

配置一个可以执行批量的sqlSession，加入到IOC容器里

#### (4)、配置事务

##### [1]、创建事务管理器

注入数据源

##### [2]、配置通知

设置所有方法都是事务方法

##### [3]、配置切入点和切面

将通知加入到service层所有方法

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx.xsd
       http://www.springframework.org/schema/aop
       https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 1、不扫描Controller注解 -->
    <context:component-scan base-package="ningct.ssm">
        <context:exclude-filter type="annotation"
                                expression="org.springframework.stereotype.Controller" />
    </context:component-scan>

    <!-- 2、Spring的配置文件，这里主要配置和业务逻辑有关的 -->
    <!--=================== 数据源，事务控制，xxx ========================-->
    <context:property-placeholder location="classpath:jdbc.properties" ignore-unresolvable="true"/>
    <bean id="pooledDataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="user" value="${jdbc.user}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>



    <!--========================== 配置和MyBatis的整合============================= -->
    <!-- 1、创建sqlSessionFactory工厂对象 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 指定mybatis全局配置文件的位置 -->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <!-- 指定数据源 -->
        <property name="dataSource" ref="pooledDataSource"/>
        <!-- 指定mapper配置文件-->
        <property name="mapperLocations" value="classpath:ningct/ssm/**/*.xml"/>
    </bean>

    <!-- 2、配置扫描器，将mybatis接口的实现加入到ioc容器中 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--扫描所有mapper接口的实现，加入到ioc容器中 -->
        <property name="basePackage" value="ningct/ssm/mapper"/>
    </bean>

    <!-- 3、配置一个可以执行批量的sqlSession -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <!-- 配置SQLSession工厂 -->
        <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"/>
        <!-- 配置执行者类型为批量 -->
        <constructor-arg name="executorType" value="BATCH"/>
    </bean>



    <!-- ============================事务控制的配置 =============================-->
    <!--1 创建事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源-->
        <property name="dataSource" ref="pooledDataSource"></property>
    </bean>

    <!--2 配置通知-->
    <tx:advice id="txadvice">
        <tx:attributes>
            <!-- 所有方法都是事务方法 -->
            <tx:method name="*"/>
            <!-- 以get开头的所有方法 -->
            <tx:method name="get*" read-only="true"/>
        </tx:attributes>
    </tx:advice>

    <!--3 配置切入点和切面-->
    <aop:config> <!--配置切入点-->
        <aop:pointcut id="pt" expression="execution(* ningct.ssm.service..*(..))"/>
        <!--配置切面-->
        <aop:advisor advice-ref="txadvice" pointcut-ref="pt"/>
    </aop:config>
</beans>
```

## 7、编写mybatis核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!-- 配置驼峰命名规则 -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!-- 设置日志 -->
        <setting name="logImpl" value="STDOUT_LOGGING" />
        <!--开启延迟加载-->
        <setting name="lazyLoadingEnabled" value="true"/>
    </settings>
</configuration>
```

## 8、创建数据库

```sql
DROP DATABASE IF EXISTS ssm;
CREATE DATABASE ssm;
USE ssm;

CREATE TABLE t_emp(
	`emp_id` INT(11) PRIMARY KEY AUTO_INCREMENT,
	`emp_name` VARCHAR(255) NOT NULL,
	`emp_gender` CHAR(1) NOT NULL,
	`emp_email` VARCHAR(255),
	`dept_id` INT(11)
);

CREATE TABLE t_dept(
	`dept_id` INT(11) PRIMARY KEY AUTO_INCREMENT,
	`dept_name` VARCHAR(255) NOT NULL
);

```

## 9、mybatis逆向工程

使用mybatis的逆向工程生成对应的bean以及mapper

### a、导入mybatis generator core包

```xml
 <!-- https://mvnrepository.com/artifact/org.mybatis.generator/mybatis-generator-core -->
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.4.1</version>
        </dependency>
```

### b、创建 mbg.xml 

在当前工程目录下创建 mbg.xml 文件（与pom.xml同级目录）

#### (1)、防止生成重复代码

多次工程会导致生成的mapper映射文件出现重复代码，进而导致spring配置文件类初始化失败，需要配置此类错误

#### (2)、不添加注释

逆向工程默认给生成的类添加注释，若不想要，需要配置

#### (3)、配置数据库连接

数据库连接记得在url添加连接的数据库以及时区等信息

当不同数据库有同名表时，会使逆向工程映射紊乱，需要配置解决

#### (4)、javaBean生成的位置

若未创建包，逆向工程会自动创建

#### (5)、指定sql映射文件生成的位置

#### (6)、指定dao接口生成的位置，mapper接口

#### (7)、table指定每个表的生成策略

mgb.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>

    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!--1、防止生成重复代码-->
        <plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin"/>
        <!--2、设置生成时不添加注释 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--3、配置数据库连接 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/ssm?useSSL=false&amp;serverTimezone=UTC"
                        userId="ningct"
                        password="ning502502502">
            <!--4、解决table schema中有多个重名的表生成表结构不一致问题 -->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>

        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <!--5、指定javaBean生成的位置 -->
        <javaModelGenerator targetPackage="ningct.ssm.pojo"
                            targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!--6、指定sql映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="mapper" targetProject=".\src\main\resources\ningct\ssm">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>

        <!-- 7、指定dao接口生成的位置，mapper接口 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="ningct.ssm.mapper" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

        <!-- 8、table指定每个表的生成策略 -->
        <table tableName="t_emp" domainObjectName="Employee"/>
        <table tableName="t_dept" domainObjectName="Department"/>
    </context>
</generatorConfiguration>

```

### c、编写测试类

通过测试运行逆向工程生成mapper和类

```java
@Test
    public void mbgTest() throws Exception {
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        File configFile = new File("mbg.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
                callback, warnings);
        myBatisGenerator.generate(null);
    }
```



## 10、测试整合

spring项目推荐使用spring的单元测试，可以自动注入我们需要的组件

需要导入spring-test包

```xml
 <!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.3.14</version>
            <scope>test</scope>
        </dependency>
```

**测试代码：**

在`Department`类和`Employee`类中添加构造函数

> 构造函数要匹配测试代码

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:applicationContext.xml"})
public class MapperTest {
    @Autowired
    DepartmentMapper departmentMapper;

    @Autowired
    EmployeeMapper employeeMapper;

    @Autowired
    SqlSession sqlSession;

    @Test
    public void test1() {
        System.out.println(departmentMapper);

        // 1. 测试插入部门
        departmentMapper.insertSelective(new Department(null, "技术部"));
        departmentMapper.insertSelective(new Department(null, "开发部"));

        // 2、生成员工数据，测试员工插入
        employeeMapper.insertSelective(new Employee(null, "xjhqre", "M", "xjhqre@126.com", 1));

        // 3、批量插入多个员工；批量，使用可以执行批量操作的sqlSession。
        EmployeeMapper mapper = sqlSession.getMapper(EmployeeMapper.class);
        for (int i = 0; i < 100; i++) {
            String uid = UUID.randomUUID().toString().substring(0, 5) + i;
            mapper.insertSelective(new Employee(null, uid, "M", uid + "@126.com", 1));
        }
    }
}
```

# 二、Maven伪分布式搭建

见Maven笔记：搭建SSM整合
