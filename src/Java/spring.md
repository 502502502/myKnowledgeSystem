## 1、使用Spring框架的好处

- **轻量：**Spring 是轻量的，基本的版本大约2MB
- **控制反转：**Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们
- **面向切面的编程(AOP)：**Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开
- **容器：**Spring 包含并管理应用中对象的生命周期和配置
- **MVC框架：**Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品
- **事务管理：**Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）
- **异常处理：**Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。

## 2、对IOC有什么了解

> 什么是 Spring IOC 容器

Spring 框架的核心是 Spring 容器。容器负责创建对象，将它们装配在一起，配置它们并管理它们的完整生命周期。

容器通过读取 XML，Java 注解或 Java 代码，获取配置元数据来接收对象进行实例化，配置和组装的指令，这就是依赖注入。

依赖注入的目的并非为软件系统带来更多功能，而是为了提升组件重用的频率，并为系统搭建一个灵活、可扩展的平台。

> IOC容器有哪些

BeanFactory以及它的一系列子类，都是IOC容器

> BeanFactory 和 ApplicationContext？

Spring的核心是容器，而容器并不唯一，框架本身就提供了很多个容器的实现，大概分为两种类型：
一种是不常用的BeanFactory，这是最简单的容器，只能提供基本的DI功能；
一种就是继承了BeanFactory后派生而来的ApplicationContext(应用上下文)，它能提供更多企业级的服务，例如解析配置文本信息等等

BeanFactory和ApplicationContext是Spring的两大核心接口，而其中ApplicationContext是BeanFactory的子接口。它们都可以当做Spring的容器，Spring容器是生成Bean实例的工厂，并管理容器中的Bean。



ApplicationContext还有额外的功能

默认初始化所有的Singleton，也可以通过配置取消预初始化。

继承MessageSource，因此支持国际化。

资源访问，比如访问URL和文件。

事件机制。

同时加载多个配置文件。

以声明式方式启动并创建Spring容器。



> BeanFactory和ApplicationContext的优缺点分析：

BeanFactory的优缺点：

- 优点：应用启动的时候占用资源很少，对资源要求较高的应用，比较有优势；
- 缺点：运行速度会相对来说慢一些。而且有可能会出现空指针异常的错误，而且通过Bean工厂创建的Bean生命周期会简单一些。

ApplicationContext的优缺点：

- 优点：所有的Bean在启动的时候都进行了加载，系统运行的速度快；在系统启动的时候，可以发现系统中的配置问题。
- 缺点：把费时的操作放到系统启动中完成，所有的对象都可以预加载，缺点就是内存占用较大



> Spring装配机制

1. 在XMl中进行显示配置

1. 在Java中进行显示配置

1. 隐式的bean发现机制和自动装配
    组件扫描（component scanning）：Spring会自动发现应用上下文中所创建的bean。
    自动装配（autowiring）：Spring自动满足bean之间的依赖。

> 构造函数注入和setter 注入

依赖注入可以通过三种方式完成，即：

- 构造函数注入
- setter 注入
- 接口注入

在 Spring Framework 中，仅使用构造函数和 setter 注入



两种依赖注入的方式，并没有绝对的好坏，只是适应的场景有所不同。相比之下，设值注入有如下优点：

设值注入需要该Bean包含这些属性的setter方法
对于复杂的依赖关系，如果采用构造注入，会导致构造器国语臃肿，难以阅读。Spring在创建Bean实例时，需要同时实例化器依赖的全部实例，因而导致性能下降。而使用设值注入，则能避免这些问题
尤其是在某些属性可选的情况况下，多参数的构造器显得更加笨重



构造注入有以下优势：
构造注入可以在构造器中决定依赖关系的注入顺序，优先依赖的优先注入。
对于依赖关系无需变化的Bean，构造注入更有用处。因为没有Setter方法，所有的依赖关系全部在构造器内设定。因此，无需担心后续的代码对依赖关系产生破坏。
对组件的调用者而言，组件内部的依赖关系完全透明，更符合高内聚的原则。



## 3、 对Spring 中的 bean有什么了解

> 作用域

- singleton : 唯一 bean 实例，Spring 中的 bean 默认都是单例的。
- prototype : 每次请求都会创建一个新的 bean 实例。
- request : 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
- session : ：在一个HTTP Session中，一个Bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。
- global-session： 全局session作用域

> 注解

- @Component ：通用的注解，可标注任意类为 Spring 组件。如果一个Bean不知道属于哪个层，可以使用@Component 注解标注。 8 @Repository : 对应持久层即 Dao 层，主要用于数据库相关操作。
- @Service : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao层。
- @Controller : 对应 Spring MVC 控制层，主要用户接受用户请求并调用 Service 层返回数据给前端页面。

> 生命周期

Bean的生命周期是由容器来管理的

创建过程：首先实例化Bean，并设置Bean的属性，根据其实现的Aware接口（主要是BeanFactoryAware接口，BeanFactoryAware，ApplicationContextAware）设置依赖信息， 接下来调用BeanPostProcess的postProcessBeforeInitialization方法，完成initial前的自定义逻辑；afterPropertiesSet方法做一些属性被设定后的自定义的事情;调用Bean自身定义的init方法，去做一些初始化相关的工作;然后再调用postProcessAfterInitialization去做一些bean初始化之后的自定义工作。这四个方法的调用有点类似AOP。 此时，Bean初始化完成，可以使用这个Bean了。 

销毁过程：如果实现了DisposableBean的destroy方法，则调用它，如果实现了自定义的销毁方法，则调用之。

> 装配策略

当 bean 在 Spring 容器中组合在一起时，它被称为装配或 bean 装配。

 Spring 容器需要知道需要什么 bean 以及容器应该如何使用依赖注入来将 bean 绑定在一起，同时装配 bean。

自动装配的不同模式：

- **no** - 这是默认设置，表示没有自动装配。应使用显式 bean 引用进行装配。
- **byName** - 它根据 bean 的名称注入对象依赖项。它匹配并装配其属性与 XML 文件中由相同名称定义的 bean。
- **byType** - 它根据类型注入对象依赖项。如果属性的类型与 XML 文件中的一个 bean 名称匹配，则匹配并装配属性。
- **构造函数** - 它通过调用类的构造函数来注入依赖项。它有大量的参数。
- **autodetect** - 首先容器尝试通过构造函数使用 autowire 装配，如果不能，则尝试通过 byType 自动装配。

> 同名bean

- 同一个配置文件内同名的Bean，以最上面定义的为准
- 不同配置文件中存在同名Bean，后解析的配置文件会覆盖先解析的配置文件
- 同文件中ComponentScan和@Bean出现同名Bean。同文件下@Bean的会生效，@ComponentScan扫描进来不会生效。通过@ComponentScan扫描进来的优先级是最低的，原因就是它扫描进来的Bean定义是最先被注册的~

> 内部bean

将 bean 用作另一个 bean 的属性时，才能将 bean 声明为内部 bean

>  Spring 怎么解决bean的循环依赖问题？

spring对循环依赖的处理有三种情况： 

①构造器的循环依赖：这种依赖spring是处理不了的，直 接抛出BeanCurrentlylnCreationException异常。

 ②单例模式下的setter循环依赖：通过“三级缓存”处理循环依赖。 

③非单例循环依赖：无法处理

![image-20230312213415808](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20230312213415808.png)

> 单例 bean 的线程安全问题？

在spring中无状态的Bean适合用不变模式，就是单例模式，这样可以共享实例提高性能。有状态的Bean在多线程环境下不安全，适合用Prototype原型模式。 

Spring使用ThreadLocal解决线程安全问题。如果你的Bean有多种状态的话（比如 View Model 对象），就需要自行保证线程安全 。



## 4、对AOP有什么了解

> 代理模式

AOP(Aspect-Oriented Programming), 即 **面向切面编程**，通过代理模式实现；

- 静态代理 - 指使用 AOP 框架提供的命令进行编译，从而在编译阶段就可生成 AOP 代理类，因此也称为编译时增强；
  - 编译时编织（特殊编译器实现）
  - 类加载时编织（特殊的类加载器实现）。
- 动态代理 - 在运行时在内存中“临时”生成 AOP 动态代理类，因此也被称为运行时增强。
  - `JDK` 动态代理：通过反射来接收被代理的类，并且要求被代理的类必须实现一个接口 。JDK 动态代理的核心是 InvocationHandler 接口和 Proxy 类 。
  - `CGLIB`动态代理： 如果目标类没有实现接口，那么 `Spring AOP` 会选择使用 `CGLIB` 来动态代理目标类 。`CGLIB` （ Code Generation Library ），是一个代码生成的类库，可以在运行时动态的生成某个类的子类，注意， `CGLIB` 是通过继承的方式做的动态代理，因此如果某个类被标记为 `final` ，那么它是无法使用 `CGLIB` 做动态代理的。

> Spring AOP and AspectJ AOP 有什么区别？

Spring AOP 基于动态代理方式实现；AspectJ 基于静态代理方式实现。 Spring AOP 仅支持方法级别的 PointCut；提供了完全的 AOP 支持，它还支持属性级别的 PointCut。



## 5、Spring 框架中用到了哪些设计模式

**工厂设计模式** : Spring使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。

**代理设计模式** : Spring AOP 功能的实现。

**单例设计模式** : Spring 中的 Bean 默认都是单例的。

**模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。

**观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。

**适配器模式** :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。



## 6、Spring 事务了解多少

> 实现方式

- 编程式事务管理：这意味着你可以通过编程的方式管理事务，这种方式带来了很大的灵活性，但很难维护。
- 声明式事务管理：这种方式意味着你可以将事务管理和业务代码分离。你只需要通过注解或者XML配置管理事务。

> 优点

- 它提供了跨不同事务api（如JTA、JDBC、Hibernate、JPA和JDO）的一致编程模型。
- 它为编程事务管理提供了比JTA等许多复杂事务API更简单的API。
- 它支持声明式事务管理。

> 传播规则

- PROPAGATION_**REQUIRED**: 支持当前事务，如果当前没有事务，就新建一个事务。这是最常见的选择。
- PROPAGATION_**SUPPORTS**: 支持当前事务，如果当前没有事务，就以非事务方式执行。
- PROPAGATION_**MANDATORY**: 支持当前事务，如果当前没有事务，就抛出异常。
- PROPAGATION_**REQUIRES_NEW**: 新建事务，如果当前存在事务，把当前事务挂起。
- PROPAGATION_**NOT_SUPPORTED**: 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起。
- PROPAGATION_**NEVER**: 以非事务方式执行，如果当前存在事务，则抛出异常。
- PROPAGATION_**NESTED**:如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则进行与PROPAGATION_REQUIRED类似的操作。



## 7、SpringMVC 了解吗

> 工作原理

![image-20230313100938544](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20230313100938544.png)

> 核心组件

| 组件                        | 说明                                                         |
| --------------------------- | ------------------------------------------------------------ |
| DispatcherServlet           | Spring MVC 的核心组件，是请求的入口，负责协调各个组件工作    |
| MultipartResolver           | 内容类型( `Content-Type` )为 `multipart/*` 的请求的解析器，例如解析处理文件上传的请求，便于获取参数信息以及上传的文件 |
| HandlerMapping              | 请求的处理器匹配器，负责为请求找到合适的 `HandlerExecutionChain` 处理器执行链，包含处理器（`handler`）和拦截器们（`interceptors`） |
| HandlerAdapter              | 处理器的适配器。因为处理器 `handler` 的类型是 Object 类型，需要有一个调用者来实现 `handler` 是怎么被执行。Spring 中的处理器的实现多变，比如用户处理器可以实现 Controller 接口、HttpRequestHandler 接口，也可以用 `@RequestMapping` 注解将方法作为一个处理器等，这就导致 Spring MVC 无法直接执行这个处理器。所以这里需要一个处理器适配器，由它去执行处理器 |
| HandlerExceptionResolver    | 处理器异常解析器，将处理器（ `handler` ）执行时发生的异常，解析( 转换 )成对应的 ModelAndView 结果 |
| RequestToViewNameTranslator | 视图名称转换器，用于解析出请求的默认视图名                   |
| LocaleResolver              | 本地化（国际化）解析器，提供国际化支持                       |
| ThemeResolver               | 主题解析器，提供可设置应用整体样式风格的支持                 |
| ViewResolver                | 视图解析器，根据视图名和国际化，获得最终的视图 View 对象     |
| FlashMapManager             | FlashMap 管理器，负责重定向时，保存参数至临时存储（默认 Session） |

> @Controller

标记一个类为 Spring Web MVC 控制器 Controller。Spring MVC 会将扫描到该注解的类，然后扫描这个类下面带有 @RequestMapping 注解的方法，根据注解信息，为这个方法生成一个对应的处理器对象

还可以实现 Spring MVC 提供的 Controller 或者 HttpRequestHandler 接口，对应的实现类也会被作为一个处理器对象

> @RequestMapping

配置处理器的 HTTP 请求方法，URI等信息，这样才能将请求和方法进行映射。这个注解可以作用于类上面，也可以作用于方法上面，在类上面一般是配置这个控制器的 URI 前缀

> @RestController

适合目前前后端分离的架构下，提供 Restful API ，返回例如 JSON 数据格式

> @GetMapping

仅可注册在方法上，仅可获取get请求

>  @RequestParam 和 @PathVariable

两个注解都用于方法参数，获取参数值的方式不同，@RequestParam 注解的参数从请求携带的参数中获取，而 @PathVariable 注解从请求的 URI 中获取

> 返回 JSON 格式

使用 @ResponseBody 注解，或者使用包含 @ResponseBody 注解的 @RestController 注解。

> springmvc拦截器

当你希望将特定功能应用于某些请求时，例如，检查用户主题时，这些拦截器非常有用。拦截器必须实现org.springframework.web.servlet包的HandlerInterceptor。此接口定义了三种方法：

preHandle：在执行实际处理程序之前调用。
postHandle：在执行完实际程序之后调用。
afterCompletion：在完成请求后调用。



## 8、对REST 的了解

> 理解

REST描述的是在网络中client和server的一种交互形式；

用URL定位资源，用HTTP描述操作

看Url就知道要什么

看http method就知道干什么

> 安全的 REST 操作

是否安全的界限，在于是否修改服务端的资源

PUT、POST 和 DELETE 是不安全的，因为他们能修改服务端的资源

> 无状态

REST API 中的请求应该包含处理它所需的所有细节。它不应该依赖于以前或下一个请求或服务器端维护的一些数据



## 9、对SpringBoot有什么了解

> 理解

在使用Spring框架进行开发的过程中，需要配置很多**Spring框架包的依赖**，如spring-core、spring-bean、spring-context等，而这些**配置**通常都是重复添加的，而且需要做很多框架使用及**环境参数**的重复配置，如开启注解、配置日志等。Spring Boot致力于弱化这些不必要的操作，**提供默认配置**，当然这些默认配置是可以按需修改的，快速搭建、开发和运行Spring应用。

> 好处

自动配置，使用基于类路径和应用程序上下文的智能默认值，当然也可以根据需要重写它们以满足开发人员的需求。
创建Spring Boot Starter 项目时，可以选择选择需要的功能，Spring Boot将为你管理依赖关系。
在开发web应用程序时，springboot会配置一个嵌入式Tomcat服务器，以便它可以作为独立的应用程序运行。

> 不同环境的属性配置文件

创建application-{profile}.properties文件，其中{profile}是具体的环境标识名称

在application.properties文件中添加spring.profiles.active=dev

> 核心注解@SpringBootApplication

@SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。

@EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项

@ComponentScan：Spring组件扫描。

> 如何理解Starters

Starters可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包

> Starter 的工作原理

 Spring Boot 在启动的时候，按照约定去读取 Spring Boot Starter 的配置信息，再根据配置信息对资源进行初始化，并注入到 Spring 容器中。



## 9、Spring 、Spring Boot 和 Spring Cloud 的关系

Spring 最初最核心的两大核心功能 Spring Ioc 和 Spring Aop 成就了 Spring，Spring 在这两大核心的功能上不断的发展，才有了 Spring 事务、Spring Mvc 等一系列伟大的产品

Spring Boot 是在强大的 Spring 帝国生态基础上面发展而来，发明 Spring Boot 不是为了取代 Spring ,是为了让人们更容易的使用 Spring 

Spring Cloud 是一系列框架的有序集合。它利用 Spring Boot 的开发便利性巧妙地简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、断路器、数据监控等，都可以用 Spring Boot 的开发风格做到一键启动和部署。