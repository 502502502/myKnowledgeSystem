 

# 一、简介

SpringCloud是分布式微服务交媾的一站式解决方案，是多种微服务交媾落地技术的集合体，俗称微服务全家桶



基于springboot实现的微服务开发，因此须严格按照官网要求选择对应的SpringBoot和SpringCloud版本





# 二、服务注册中心

## 1、Eureka

### ①、基础知识

当服务很多时,单靠代码手动管理是很麻烦的,需要一个公共组件,统一管理多服务之间的相互调用。



**服务治理**　　

Spring Cloud 封装了 Netflix 公司开发的 Eureka 模块来实现服务治理

在传统的rpc远程调用框架中，管理每个服务与服务之间依赖关系比较复杂，管理比较复杂，所以需要使用服务治理，管理服务于服务之间依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现与注册。



**服务注册与发现**

如果你的服务注册中心只有一台机器，那么只要它一宕机，就会产生单点故障。所以一般服务注册中心能配多个就配多个，主要就是为了避免单点故障。

Eureka采用了CS(Client-Server)的设计架构，Eureka Server 作为服务注册功能的服务器，它是服务注册中心。而系统中的其他微服务，使用 Eureka的客户端连接到 Eureka Server并维持心跳连接。这样系统的维护人员就可以通过 Eureka Server 来监控系统中各个微服务是否正常运行。

在服务注册与发现中，有一个注册中心。当服务器启动的时候，会把当前自己服务器的信息 比如 服务地址通讯地址等以别名方式注册到注册中心上。另一方（消费者|服务提供者），以该别名的方式去注册中心上获取到实际的服务通讯地址，然后再实现本地RPC调用RPC远程调用框架核心设计思想：在于注册中心，因为使用注册中心管理每个服务与服务之间的一个依赖关系(服务治理概念)。在任何rpc远程框架中，都会有一个注册中心(存放服务地址相关信息(接口地址))



**Eureka Server提供服务注册服务**

各个微服务节点通过配置启动后，会在EurekaServer中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。

  

**EurekaClient通过注册中心进行访问**

是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置的、使用轮询(round-robin)负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳(默认周期为30秒)。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除（默认90秒）



### ②、单机Eureka



#### Ⅰ、server

Eureka Server 本身也是一个微服务

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220726152111132.png" alt="image-20220726152111132" style="zoom:33%;" />

pom

```xml
1.x:    server跟client合在一起
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
  </dependency>
2.x： server跟client分开
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
  </dependency>
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
  </dependency>
```

yml

```yaml
server:
  port: 7001

eureka:
  instance:
    hostname: localhost #eureka服务端的实例名称
  client:
    #false表示不向注册中心注册自己。
    register-with-eureka: false
    #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
  service-url:
    #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址。
    defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/

```

main

```java
@SpringBootApplication
@EnableEurekaServer  //表示当前是Eureka的服务注册中心
public class EurekaMain7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaMain7001.class, args);
    }
}
```



访问 http://localhost:7001



#### Ⅱ、client

pom

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

```

yml

```yaml
eureka:
  client:
    # 表示将自己注册进EurekaServer默认为true
    register-with-eureka: true
    # 表示可以从Eureka抓取已有的注册信息，默认为true。单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetch-registry: true
    service-url: 
      defaultZone: http://localhost:7001/eureka

```

main

```java
@SpringBootApplication
@EnableEurekaClient
@EnableDiscoveryClient
//@MapperScan("com.atguigu.springcloud.dao")
public class PaymentMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class, args);
    }
}
```



### ③、集群Eureka

#### Ⅰ、集群server

Eureka集群原理：互相注册，相互守望

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220726152808022.png" alt="image-20220726152808022" style="zoom:33%;" />

yaml

```yaml
server:
  port: 7001

eureka:
  instance:
    hostname: eureka7001.com #eureka服务端的实例名称 这里跟host配置文件中的一致
  client:
    #false表示不向注册中心注册自己。
    register-with-eureka: false
    #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    fetch-registry: false
    service-url:
    #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址。
      # 单机就是自己
      # defaultZone: http://eureka7001.com:7001/eureka/
      # 集群指向其他eureka
      #defaultZone: http://eureka7002.com:7002/eureka/
      #写成这样可以直接通过可视化页面跳转到7002
      defaultZone: http://eureka7002.com:7002/
```



#### Ⅱ、集群client

多个provider微服务在注册中心中的注册名是一样的，这两个微服务对外暴露的都是同一个名字，

即yml文件中spring.application.name相同



yaml

```yaml
defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka #集群版
```



#### Ⅲ、负载均衡

调用方使用@LoadBlanced注解赋予RestTemplate负载均衡的能力



### ④、其它

#### Ⅰ、actuator微服务信息完善

只暴露服务名，不带有主机名

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220726154726176.png" alt="image-20220726154726176" style="zoom:33%;" />



访问信息有IP信息提示

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220726154803811.png" alt="image-20220726154803811" style="zoom:33%;" />



#### Ⅱ、服务发现Discovery

**服务发现**：对于注册进eureka里面的微服务，可以通过服务发现来获得该服务的信息

**@EnableDiscoveryClient**注解



#### Ⅲ、eureka自我保护

保护模式主要用于一组客户端和EurekaServer之间存在网络分区场景下的保护。一旦进入保护模式， EurekaServer将会尝试保护其服务注册表中的信息，不在删除服务注册表中的数据，也就是不会注销任何微服务。 

一句话：某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对该微服务的信息进行保存。
属于CAP里面的AP分支。



**禁止自我保护**

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220726155055933.png" alt="image-20220726155055933" style="zoom: 50%;" />

<img src="C:/Users/ning chuang tao/AppData/Roaming/Typora/typora-user-images/image-20220726155124700.png" alt="image-20220726155124700" style="zoom:50%;" />



## 2、Zookeeper

### ①、启动zookeeper

在Linux上安装zookeeper并启动



### ②、微服务注册

pom

```xml
 <!-- SpringBoot整合zookeeper客户端 -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
        </dependency>
```

yml

```yaml
#8004表示注册到zookeeper服务器的支付服务提供者端口号
server:
  port: 8004

#服务别名----注册zookeeper到注册中心名称
spring:
  application:
    name: cloud-provider-payment
  cloud:
    zookeeper:
      connect-string: 192.168.190.128:2181
```

main

```java
@SpringBootApplication
@EnableDiscoveryClient //该注解用于向使用consul或者zookeeper作为注册中心时注册服务
public class PaymentMain8004 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8004.class, args);
    }
}
```



## 3、Consul

基于raft协议，比较简洁；支持健康检查，同时支持HTTP和DNS协议支持跨数据中心的WAN集群。

提供图形界面 跨平台，支持Linux，MAC，Windows

### ①、安装运行

### ②、注册服务

pom

```xml
 		<!--SpringCloud consul-server -->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-consul-discovery</artifactId>
        </dependency>
```

yml

```yaml
###consul服务端口号
server:
  port: 8006

spring:
  application:
    name: consul-provider-payment
  ####consul注册中心地址
  cloud:
    consul:
      host: localhost
      port: 8500
      discovery:
        service-name: ${spring.application.name}
```



### ③、访问可视化



# 三、服务调用

## 1、Ribbon

### ①、负载均衡

**LB 负载均衡 Load balance 是什么**

简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA（高可用）

常见的负载均衡有软件  Nginx   LVS   硬件F5等



**常见的负载均衡算法**

- **轮询**：为第一个请求选择健康池中的第一个后端服务器，然后按顺序往后依次选择，直到最后一个，然后循环。

- **最小连接**：优先选择连接数最少，也就是压力最小的后端服务器，在会话较长的情况下可以考虑采取这种方式。

- **散列**：根据请求源的 IP 的散列（hash）来选择要转发的服务器。这种方式可以一定程度上保证特定用户能连接到相同的服务器。如果你的应用需要处理状态而要求用户能连接到和之前相同的服务器，可以考虑采取这种方式。



### ②、原理

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220727172120268.png" alt="image-20220727172120268" style="zoom:50%;" />

### ③、依赖

spring-boot-netfix-eureka-client自带了spring-starter-ribbon引用



### ④、服务调用

Ribbon结合RestTemplate实现调用微服务之间的调用

```java
------------------ getForObject() --------------------------------------
@GetMapping("/consumer/payment/get/{id}")
public CommonResult<Payment> getPayment(@PathVariable("id") Long id) {
    // 返回对象为响应体中数据转化成的对象，基本上可以理解为JSO
    return restTemplate.getForObject(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);
}

------------------ getForEntity() --------------------------------------
/**
 * 测试getForEntity   cloud-consumer-order80模块，OrderController类中
 */
@GetMapping("/consumer/payment/getForEntity/{id}")
public CommonResult<Payment> getPayment2(@PathVariable("id") Long id) {
    // 返回对象为ResponseEntity对象，包含了响应中的一些重要信息，比如响应头，响应状态码，响应体等。
    ResponseEntity<CommonResult> entity =  restTemplate.getForEntity(PAYMENT_URL + "/payment/get/" + id, CommonResult.class);
    if (entity.getStatusCode().is2xxSuccessful()) {
        log.info(entity.getStatusCode() + "\t" + entity.getHeaders());
        return entity.getBody();
    }else {
        return new CommonResult<>(444, "操作失败");
    }
}
```



### ⑤、IRule接口

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/1632148745291-f05e92c8-8e23-46b8-8d7a-72ff6cafb14f.png" alt="image.png" style="zoom: 50%;" />

- **com.netflix.loadbalancer.RoundRobinRule**：轮询
- **com.netflix.loadbalancer.RandomRule**：随机
- **com.netflix.loadbalancer.RetryRule**：先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重试
- **WeightedResponseTimeRule**：对RoundRobinRule的扩展，响应速度越快的实例选择权重越大，越容易被选择
- **BestAvailableRule**：会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务
- **AvailabilityFilteringRule**：先过滤掉故障实例，再选择并发较小的实例
- **ZoneAvoidanceRule**：默认规则，复合判断server所在区域的性能和server的可用性选择服务器



### ⑥、替换负载均衡算法

**修改调用方**



**新建配置类**

```java
@Configuration
public class MySelfRule
{
    @Bean
    public IRule myRule()
    {
        return new RandomRule();//定义为随机负载均衡算法
    }
}
```

> 自定义负载均衡配置类不能放在@ComponentScan所扫描的当前包下以及子包下，否则我们自定义的这个配置类就会被所有的Ribbon客户端所共享，达不到特殊化定制的目的了。



**主启动类添加@RibbonCilent注解**

```java
//指明访问的服务CLOUD-PAYMENT-SERVICE，以及指定负载均衡策略
@RibbonClient(name = "CLOUD-PAYMENT-SERVICE",configuration= MySelfRule.class)
public class OrderMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderMain80.class, args);
    }
}
```



### ⑦、算法原理

轮循负载均衡算法：rest接口第几次请求数 % 服务器集群总数量 = 实际调用服务器位置下标，每次服务重启后rest接口计数从1开始。



## 2、OpenFeign

### ①、概述

只需创建一个接口并使用注解的方式来配置它(以前是Dao接口上面标注Mapper注解,现在是一个微服务接口上面标注一个Feign注解即可)，即可完成对服务提供方的接口绑定，简化了使用Spring cloud Ribbon时，自动封装服务调用客户端的开发量。



**Feign集成了Ribbon**

利用Ribbon维护了Payment的服务列表信息，并且通过轮询实现了客户端的负载均衡。而与Ribbon不同的是，通过feign只需要以声明式的方法定义服务且绑定接口，优雅而简单的实现了服务调用



### ②、使用步骤

**依赖**

```xml
		<!--openfeign   新增-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
```

**不注册**

```yaml
server:
  port: 80

eureka:
  client:
    # 表示不将其注入Eureka作为微服务，不作为Eureak客户端了，而是作为Feign客户端
    register-with-eureka: false
    service-url:
      # 集群版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka,http://eureka7003.com:7003/eureka

```

**主启动**

```java
@EnableFeignClients //不作为Eureak客户端了，而是作为Feign客户端
public class OrderOpenFeignMain80 {
    public static void main(String[] args) {
        SpringApplication.run(OrderOpenFeignMain80.class, args);
    }

}
```

**service层**

- 方法签名，必须和provider微服务（服务提供者微服务）中的controller中方法的签名一致
- 如果需要传递参数，那么`@RequestParam` 和`@RequestBody` `@PathVariable` 不能省 必加

```java
@Component
@FeignClient(value = "CLOUD-PAYMENT-SERVICE") //作为一个Feign功能绑定的的接口
public interface PaymentFeignService {
    @GetMapping(value = "/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id);
}
```



### ③、超时控制

Feign 默认是支持Ribbon ，Feign依赖里自己带了Ribbon；Feign客户端的负载均衡和超时控制都由Ribbon控制



**修改yml**

```yaml
#设置feign客户端超时时间(OpenFeign默认支持ribbon)
ribbon:
  #指的是建立连接后从服务器读取到可用资源所用的时间
  ReadTimeout: 5000
  #指的是建立连接所用的时间，适用于网络状况正常的情况下,两端连接所用的时间
  ConnectTimeout: 5000
```



### ④、日志打印功能

**日志级别**

| NONE    | 默认的，不显示任何日志                                    |
| ------- | --------------------------------------------------------- |
| BASIC   | 仅记录请求方法、URL、响应状态码及执行时间                 |
| HEADERS | 除了BASIC中定义的信息之外，还有请求和响应的头信息         |
| FULL    | 除了HEADERS中定义的信息外，还有请求和响应的正文及元数据。 |

**注入bean**

```java
@Configuration
public class FeignConfig {
    @Bean
    Logger.Level feignLoggerLevel()
    {
        return Logger.Level.FULL;
    }
}
```

**yml配置**

```yaml
logging:
  level:
    # feign 日志以什么级别监控哪个接口
    com.atguigu.springcloud.service.PaymentFeignService: debug
```



# 四、服务降级

### ①、服务雪崩

多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其他的微服务，这就是所谓的“扇出”如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”



### ②、Hystrix 

Hystrix 是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。



### ③、服务降级：fallback

加入对方的系统不可用了，需要一个兜底的解决方案或备选响应；向调用方返回一个符合预期的、可处理的备选响应。

服务器繁忙，请稍后再试，不让客户端等待并立刻返回一个好友提示。`fallback`

哪些情况会触发降级：

- 程序运行异常
- 超时
- 服务熔断触发服务降级
- 线程池/信号量打满也会导致服务降级



### ④、服务熔断：break

类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并返回友好提示 `break`

熔断状态： 开启   关闭    半开启

<img src="https://cdn.nlark.com/yuque/0/2021/png/22423156/1632729761132-5a6fe723-723a-4674-8442-5ea5b742aacb.png" alt="image.png" style="zoom: 80%;" />



**熔断类型**

- 熔断打开：请求不再进行调用当前服务，再有请求调用时将不会调用主逻辑，而是直接调用降级fallback。实现了自动的发现错误并将降级逻辑切换为主逻辑，减少响应延迟效果。内部设置时钟一般为MTTR（Mean time to repair，平均故障处理时间)，当打开时长达到所设时钟则进入半熔断状态。
- 熔断关闭：熔断关闭不会对服务进行熔断，服务正常调用
- 熔断半开：部分请求根据规则调用当前服务，如果请求成功且符合规则则认为当前服务恢复正常，关闭熔断



断路器的四个重要参数：**快照时间窗、请求总数阀值、窗口睡眠时间、错误百分比阀值**。

- **快照时间窗（滚动窗口****）**：断路器确定是否打开需要统计一些请求和错误数据，而统计的时间范围就是快照时间窗，默认为最近的10秒。  

- - **metrics.rollingStats.timeInMilliseconds**

- **请求总数阀值**：在快照时间窗内，必须满足请求总数阀值才有资格熔断。默认为20，意味着在10秒内，如果该hystrix命令的调用次数不足20次，即使所有的请求都超时或其他原因失败，断路器都不会打开。

- - **circuitBreaker.requestVolumeThreshold**

- **窗口睡眠时间**：剩下一个表示窗口睡眠时间，即断路器触发多少秒（默认5s）后尝试恢复，进入半开状态。

- - **circuitBreaker.sleepWindowInMilliseconds**

- **错误百分比阀值**：当请求总数在快照时间窗内超过了阀值，比如发生了30次调用，如果在这30次调用中，有15次发生了超时异常，也就是超过50%的错误百分比，在默认设定50%阀值情况下，这时候就会将断路器打开。

- - **circuitBreaker.errorThresholdPercentage**



**断路器开启或关闭条件**

+ 当满足一定的阈值的时候（默认10秒内超过20个请求次数）；
+ 当失败率达到一定的时候（默认10秒内超过50%的请求失败）；
+ 到达以上阈值，断路器将会开启；
+ 当开启的时候，所有请求都不会进行转发；
+ 一段时间之后（默认是5秒），这个时候断路器是半开状态，会让其中一个请求进行转发；如果成功，断路器会关闭，若失败，继续开启。重复4和5。



**断路器打开之后**

1： 再有请求调用的时候，还会调用主逻辑吗？

将不会调用主逻辑，而是直接调用降级的fallback方法，通过断路器，实现了自动的发现错误并将降级逻辑升级为主逻辑，减少响应延迟的效果。



2：原来的主逻辑要如何恢复？

- 对于这一问题mhystrix也为我们实现了自动恢复功能。
- 当断路器打开，对主逻辑进行熔断之后，hystrix会启动一个休眠时间窗，在这个时间窗内，降级逻辑是临时的成为主逻辑。
- 当休眠时间窗到期，断路器将进入半开状态，释放给一次请求到原来的主逻辑上，如果此次请求正常返回，那么断路器将继续闭合。
- 主逻辑恢复，如果这次请求依然有问题，断路器继续进入打开状态，休眠时间窗重新计时。



### ⑤、服务限流：flowlimit

秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒钟N个，有序进行  `flowlimit`





### ⑥、服务降级使用

#### Ⅰ、服务端

**pom**

```xml
		<!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
```

**service**

```java
//模拟拥堵的情况
@HystrixCommand(fallbackMethod = "paymentInfo_TimeoutHandler", commandProperties = {
    //规定这个线程的超时时间是3s，3s后就由fallbackMethod指定的方法帮我“兜底”（服务降级）
    @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds", value = "3000")
})
public String paymentInfo_Timeout(Integer id) {
    int timeNumber = 5;
    //int age = 10 / 0;
    try {
        TimeUnit.SECONDS.sleep(timeNumber);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    return "线程池：  " + Thread.currentThread().getName() + "   payment_Timeout,  id:  " + id
        + "\t"+"O(∩_∩)O哈哈~耗时(s):" + timeNumber;
}

public String paymentInfo_TimeoutHandler(Integer id) {
    return "线程池：  "+ Thread.currentThread().getName()+"   paymentInfo_TimeoutHandler,  id:  "+id
        +"\t"+"o(╥﹏╥)o";
}
```

#### Ⅱ、客户端

**pom**

```xml
<!--hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>
```

**yml**

```yaml
feign:
  hystrix:
    enabled: true
```

**controller**

```java
 @GetMapping("/consumer/payment/hystrix/timeout/{id}")
    @HystrixCommand(fallbackMethod = "paymentTimeOutFallbackMethod",commandProperties = {
            @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",
                    value="1500")
    })
    public String paymentInfo_Timeout(@PathVariable("id") Integer id) {
        return paymentHystrixService.paymentInfo_Timeout(id);
    }

    public String paymentTimeOutFallbackMethod(@PathVariable("id") Integer id)
    {
        return "我是消费者80,对方支付系统繁忙请10秒钟后再试或者自己运行出错请检查自己,o(╥﹏╥)o";
    }
```



#### Ⅲ、解耦优化

为Feign客户端定义的接口添加一个服务降级实现类即可实现解耦

**继承service**

```java
@Component  //不要忘记这个注解
public class PaymentFallbackService implements PaymentHystrixService{

    @Override
    public String paymentInfo_Ok(Integer id) {
        return "------PaymentFallbackService-paymentInfo_Ok, fallback";
    }

    @Override
    public String paymentInfo_Timeout(Integer id) {
        return "------PaymentFallbackService-paymentInfo_Timeout, fallback";
    }
}
```

**service**

```java
@Component
@FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT", fallback = PaymentFallbackService.class)
public interface PaymentHystrixService {
    @GetMapping("/payment/hystrix/ok/{id}")
    public String paymentInfo_Ok(@PathVariable("id") Integer id);

    @GetMapping("/payment/hystrix/timeout/{id}")
    public String paymentInfo_Timeout(@PathVariable("id") Integer id);
}
```



### ⑦、服务熔断使用

#### Ⅰ、服务端

**service**

```java
//=============================服务熔断===============================
@HystrixCommand(fallbackMethod = "paymentCircuitBreaker_fallback",commandProperties = {
    @HystrixProperty(name = "circuitBreaker.enabled",value = "true"), //是否开启断路器
    @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"),  //最小请求次数
    //短路多久以后开始尝试是否恢复，默认5s
    @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"), 
    @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60")  //失败率达到多少后跳闸
})   //总的意思就是在n毫秒内的时间窗口期内，m次请求中有p%的请求失败了，那么断路器启动
public String paymentCircuitBreaker(@PathVariable("id") Integer id)
{
    if(id < 0)
    {
        throw new RuntimeException("******id 不能负数");
    }
    String serialNumber = IdUtil.simpleUUID();  //等价于UUID.randomUUID().toString()

    return Thread.currentThread().getName()+"\t"+"调用成功，流水号: " + serialNumber;
}
public String paymentCircuitBreaker_fallback(@PathVariable("id") Integer id)
{
    return "id 不能负数，请稍后再试，/(ㄒoㄒ)/~~   id: " +id;
}
```

```java
//========================All
@HystrixCommand(fallbackMethod = "str_fallbackMethod",
        groupKey = "strGroupCommand",
        commandKey = "strCommand",
        threadPoolKey = "strThreadPool",

        commandProperties = {
                // 设置隔离策略，THREAD 表示线程池 SEMAPHORE：信号池隔离
                @HystrixProperty(name = "execution.isolation.strategy", value = "THREAD"),
                // 当隔离策略选择信号池隔离的时候，用来设置信号池的大小（最大并发数）
                @HystrixProperty(name = "execution.isolation.semaphore.maxConcurrentRequests", value = "10"),
                // 配置命令执行的超时时间
                @HystrixProperty(name = "execution.isolation.thread.timeoutinMilliseconds", value = "10"),
                // 是否启用超时时间
                @HystrixProperty(name = "execution.timeout.enabled", value = "true"),
                // 执行超时的时候是否中断
                @HystrixProperty(name = "execution.isolation.thread.interruptOnTimeout", value = "true"),
                // 执行被取消的时候是否中断
                @HystrixProperty(name = "execution.isolation.thread.interruptOnCancel", value = "true"),
                // 允许回调方法执行的最大并发数
                @HystrixProperty(name = "fallback.isolation.semaphore.maxConcurrentRequests", value = "10"),
                // 服务降级是否启用，是否执行回调函数
                @HystrixProperty(name = "fallback.enabled", value = "true"),
                // 是否启用断路器
                @HystrixProperty(name = "circuitBreaker.enabled", value = "true"),
                // 该属性用来设置在滚动时间窗中，断路器熔断的最小请求数。例如，默认该值为 20 的时候，
                // 如果滚动时间窗（默认10秒）内仅收到了19个请求， 即使这19个请求都失败了，断路器也不会打开。
                @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold", value = "20"),
                // 该属性用来设置在滚动时间窗中，表示在滚动时间窗中，在请求数量超过
                // circuitBreaker.requestVolumeThreshold 的情况下，如果错误请求数的百分比超过50,
                // 就把断路器设置为 "打开" 状态，否则就设置为 "关闭" 状态。
                @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage", value = "50"),
                // 该属性用来设置当断路器打开之后的休眠时间窗。 休眠时间窗结束之后，
                // 会将断路器置为 "半开" 状态，尝试熔断的请求命令，如果依然失败就将断路器继续设置为 "打开" 状态，
                // 如果成功就设置为 "关闭" 状态。
                @HystrixProperty(name = "circuitBreaker.sleepWindowinMilliseconds", value = "5000"),
                // 断路器强制打开
                @HystrixProperty(name = "circuitBreaker.forceOpen", value = "false"),
                // 断路器强制关闭
                @HystrixProperty(name = "circuitBreaker.forceClosed", value = "false"),
                // 滚动时间窗设置，该时间用于断路器判断健康度时需要收集信息的持续时间
                @HystrixProperty(name = "metrics.rollingStats.timeinMilliseconds", value = "10000"),
                // 该属性用来设置滚动时间窗统计指标信息时划分"桶"的数量，断路器在收集指标信息的时候会根据
                // 设置的时间窗长度拆分成多个 "桶" 来累计各度量值，每个"桶"记录了一段时间内的采集指标。
                // 比如 10 秒内拆分成 10 个"桶"收集这样，所以 timeinMilliseconds 必须能被 numBuckets 整除。否则会抛异常
                @HystrixProperty(name = "metrics.rollingStats.numBuckets", value = "10"),
                // 该属性用来设置对命令执行的延迟是否使用百分位数来跟踪和计算。如果设置为 false, 那么所有的概要统计都将返回 -1。
                @HystrixProperty(name = "metrics.rollingPercentile.enabled", value = "false"),
                // 该属性用来设置百分位统计的滚动窗口的持续时间，单位为毫秒。
                @HystrixProperty(name = "metrics.rollingPercentile.timeInMilliseconds", value = "60000"),
                // 该属性用来设置百分位统计滚动窗口中使用 “ 桶 ”的数量。
                @HystrixProperty(name = "metrics.rollingPercentile.numBuckets", value = "60000"),
                // 该属性用来设置在执行过程中每个 “桶” 中保留的最大执行次数。如果在滚动时间窗内发生超过该设定值的执行次数，
                // 就从最初的位置开始重写。例如，将该值设置为100, 滚动窗口为10秒，若在10秒内一个 “桶 ”中发生了500次执行，
                // 那么该 “桶” 中只保留 最后的100次执行的统计。另外，增加该值的大小将会增加内存量的消耗，并增加排序百分位数所需的计算时间。
                @HystrixProperty(name = "metrics.rollingPercentile.bucketSize", value = "100"),
                // 该属性用来设置采集影响断路器状态的健康快照（请求的成功、 错误百分比）的间隔等待时间。
                @HystrixProperty(name = "metrics.healthSnapshot.intervalinMilliseconds", value = "500"),
                // 是否开启请求缓存
                @HystrixProperty(name = "requestCache.enabled", value = "true"),
                // HystrixCommand的执行和事件是否打印日志到 HystrixRequestLog 中
                @HystrixProperty(name = "requestLog.enabled", value = "true"),
        },
        threadPoolProperties = {
                // 该参数用来设置执行命令线程池的核心线程数，该值也就是命令执行的最大并发量
                @HystrixProperty(name = "coreSize", value = "10"),
                // 该参数用来设置线程池的最大队列大小。当设置为 -1 时，线程池将使用 SynchronousQueue 实现的队列，
                // 否则将使用 LinkedBlockingQueue 实现的队列。
                @HystrixProperty(name = "maxQueueSize", value = "-1"),
                // 该参数用来为队列设置拒绝阈值。 通过该参数， 即使队列没有达到最大值也能拒绝请求。
                // 该参数主要是对 LinkedBlockingQueue 队列的补充,因为 LinkedBlockingQueue
                // 队列不能动态修改它的对象大小，而通过该属性就可以调整拒绝请求的队列大小了。
                @HystrixProperty(name = "queueSizeRejectionThreshold", value = "5"),
        }
)
public String strConsumer() {
    return "hello 2020";
}
public String str_fallbackMethod()
{
    return "*****fall back str_fallbackMethod";
}
```



### ⑧、服务监控 HystrixDashBoard

Hystrix还提供了准实时的调用监控（Hystrix Dashboard）Hystrix会持续的记录所有通过Hystrix发起的请求的执行信息，并以统计报表和图形的形式展示给用户，包括每秒执行多少请求多少成功，多少失败等



**pom**

```xml
		<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>
 		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
```

yaml

![image.png](https://ningct.oss-cn-hangzhou.aliyuncs.com/1632792217201-11413334-f246-4cd8-a71f-d64e9e05ac3d.png)

main

```java
@EnableHystrixDashboard // 开启Hystrix仪表盘
public class HystrixDashboardMain9001 {
    public static void main(String[] args) {
        SpringApplication.run(HystrixDashboardMain9001.class, args);
    }
}
```



确保所有生产者微服务中均包含**spring-boot-starter-actuator**依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```



访问http://localhost:9001/hystrix





# 五、服务网关

## 1、Gateway概述

SpringCloud Gateway 使用的Webflux中的reactor-netty响应式编程组件，底层使用了Netty通讯框架

Gateway旨在提供一种简单而有效的方式来对API进行路由，以及提供一些强大的过滤器功能，例如：熔断、限流、重试等。

<img src="https://ningct.oss-cn-hangzhou.aliyuncs.com/1632810706535-71a5e3e9-f84c-4543-9bbe-5da3d0c070e2.png" alt="image.png" style="zoom:50%;" />



**特点**

- 基于Spring Frameword5 ,Project Reactor 和 SpringBoot 2.0进行构建；
- 动态路由：能够匹配任何请求属性
- 可以对路由指定Predicate(断言)和Filter（过滤器）
- 集成Hystrix的断路器功能；
- 集成Spring Cloud的服务发现功能
- 易于编写的Predicate（断言）和Filter（过滤器）
- 请求限流功能；
- 支持路径重写



### ②、Route(路由)

路由是构建网关的基本模块，它由ID，目标URI（Uniform Resource Identifier，统一资源标识符），一系列的断言和过滤器组成，如果断言为true则匹配该路由。



### ③、Predicate(断言)

开发人员可以匹配Http请求中的所有内容（例如请求头或者请求参数），如果请求参数与断言相匹配则进行路由。



### ④、Filter(过滤)

指的是Spring框架中的GatewayFilter的实例，使用过滤器，可以在请求被路由前或者之后对请求进行修改。



### ⑤、使用

做网关不需要添加  web starter  否则会报错

pom

```xml
 <!--gateway-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
```

yml

```yaml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
//=====================新增====================
  cloud:
    gateway:
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          uri: http://localhost:8001          #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          uri: http://localhost:8001          #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
//===================================================

eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
#      defaultZone: http://eureka7001.com:7001/eureka # 单机版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka #集群版
```

![image.png](https://ningct.oss-cn-hangzhou.aliyuncs.com/1632815268348-1e0ea7e2-df01-4364-b93e-48e5f5483fea.png)

main

```java
@SpringBootApplication
@EnableEurekaClient
public class GateWayMain9527 {
    public static void main(String[] args) {
        SpringApplication.run(GateWayMain9527.class, args);
    }
}
```



### ⑥、动态路由

yml

```yaml
spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      discovery:
        locator:
          enabled: true #开启从注册中心动态创建路由的功能，利用微服务名进行路由
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
#          uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service   #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/get/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
#          uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://cloud-payment-service  #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/lb/**         # 断言，路径相匹配的进行路由
```

> uri: lb://cloud-payment-service 
>
> lb://开头代表从注册中心中获取服务，后面接的就是你需要转发到的服务名称，而且找到的服务实现负载均衡。

# 六、服务配置

# 七、服务总线

# 八、消息驱动

# 九、链路追踪

十