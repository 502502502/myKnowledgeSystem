[TOC]



# 一、容器

## 1、AnnotationConfigApplicationContext

### ①、配置类

#### Ⅰ、新建Java类

#### Ⅱ、添加注解@Configuration

```java
/**
 * 以前配置文件的方式被替换成了配置类，即配置类==配置文件
 * @author liayun
 *
 */
// 这个配置类也是一个组件 
@Configuration // 告诉Spring这是一个配置类
public class MainConfig {
	
}
```

### ②、注入JavaBean

#### Ⅰ、新建Java类

#### Ⅱ、在配置类写方法

#### Ⅲ、在方法上添加注解@Bean

```java
// @Bean注解是给IOC容器中注册一个bean，类型自然就是返回值的类型，id默认是用方法名作为id
	@Bean
	public Person person() {
		return new Person("liayun", 20);
	}
```

> 使用注解注入JavaBean时，bean在IOC容器中的名称就是使用@Bean注解标注的方法名称

#### Ⅳ、指定bean名称

在@Bean注解中明确指定名称

```java
@Bean("person")
	public Person person01() {
		return new Person("liayun", 20);
	}
```

### ③、创建IOC容器

在使用的时候new一个IOC容器对象，参数为配置类

```java
ApplicationContext applicationContext = new
    AnnotationConfigApplicationContext(MainConfig.class);
```

### ④、从IOC容器获取对象

```java
Person person = applicationContext.getBean(Person.class);
```

### ⑤、包扫描

#### Ⅰ、为什么要扫描包

使用Spring的包扫描功能对项目中的包进行扫描，凡是在指定的包或其子包中的类上标注了@Repository、@Service、@Controller、@Component注解的类都会被扫描到，并将这个类注入到Spring容器中

#### Ⅱ、注解配置扫描包

在配置类上添加`@ComponentScan(value="com.meimeixia")`注解

```java
@ComponentScan(value="com.meimeixia") // value指定要扫描的包
@Configuration 
public class MainConfig {
}
```

#### Ⅲ、扫描排除

##### [1]、含义

除了某些组件之外，IOC容器中剩下的组件我都要，即相当于是要排除某些组件

##### [2]、代码

```java
@ComponentScan(value="com.meimeixia", excludeFilters={
		/*
		 * type：指定你要排除的规则，是按照注解进行排除，还是按照给定的类型进行排除，还是按照正则表达式进行排除，等等
		 * classes：除了@Controller和@Service标注的组件之外，IOC容器中剩下的组件我都要，即相当于是我要排除@Controller和@Service这俩注解标注的组件。
		 */
		@Filter(type=FilterType.ANNOTATION, classes={Controller.class, Service.class})
})
```

##### [3]、参数分析



#### Ⅳ、扫描只包含

##### [1]、含义

只需要某些组件

##### [2]、代码

```java
@ComponentScan(value="com.meimeixia", includeFilters={
		/*
		 * type：指定你要排除的规则，是按照注解进行排除，还是按照给定的类型进行排除，还是按照正则表达式进行排除，等等
		 * classes：我们需要Spring在扫描时，只包含@Controller注解标注的类
		 */
		@Filter(type=FilterType.ANNOTATION, classes={Controller.class})
}, useDefaultFilters=false)
```

> 使用includeFilters()方法来指定只包含哪些注解标注的类时，需要禁用掉默认的过滤规则

> 在使用includeFilters()方法来指定只包含哪些注解标注的类时，结果信息中会一同输出Spring内部的组件名称

##### [3]、参数分析

在使用@ComponentScan注解实现包扫描时，我们可以使用@Filter指定过滤规则，在@Filter中，通过type来指定过滤的类型，@Filter注解中的type属性是一个FilterType枚举，具体值如下：

###### (1)、FilterType.ANNOTATION

按照注解进行包含或者排除

```java
@ComponentScan(value="com.meimeixia", includeFilters={
		/*
		 * type：指定你要排除的规则，是按照注解进行排除，还是按照给定的类型进行排除，还是按照正则表达式进行排除，等等
		 * classes：我们需要Spring在扫描时，只包含@Controller注解标注的类
		 */
		@Filter(type=FilterType.ANNOTATION, classes={Controller.class})
}, useDefaultFilters=false)

```



###### (2)、FilterType.ASSIGNABLE_TYPE

按照给定的类型进行包含或者排除

```java
@ComponentScan(value="com.meimeixia", includeFilters={
		/*
		 * type：指定你要排除的规则，是按照注解进行排除，还是按照给定的类型进行排除，还是按照正则表达式进行排除，等等
		 */
		// 只要是BookService这种类型的组件都会被加载到容器中，不管是它的子类还是什么它的实现类。记住，只要是BookService这种类型的
		@Filter(type=FilterType.ASSIGNABLE_TYPE, classes={BookService.class})
}, useDefaultFilters=false)

```



###### (3)、FilterType.ASPECTJ

```java
@ComponentScan(value="com.meimeixia", includeFilters={
		/*
		 * type：指定你要排除的规则，是按照注解进行排除，还是按照给定的类型进行排除，还是按照正则表达式进行排除，等等
		 */
		@Filter(type=FilterType.ASPECTJ, classes={AspectJTypeFilter.class})
}, useDefaultFilters=false)

```



###### (4)、FilterType.REGEX

按照正则表达式进行包含或者排除

```java
@ComponentScan(value="com.meimeixia", includeFilters={
		/*
		 * type：指定你要排除的规则，是按照注解进行排除，还是按照给定的类型进行排除，还是按照正则表达式进行排除，等等
		 */
		@Filter(type=FilterType.REGEX, classes={RegexPatternTypeFilter.class})
}, useDefaultFilters=false) // value指定要扫描的包

```



###### (5)、FilterType.CUSTOM

按照自定义规则进行包含或者排除

```java
@ComponentScan(value="com.meimeixia", includeFilters={
				/*
				 * type：指定你要排除的规则，是按照注解进行排除，还是按照给定的类型进行排除，还是按照正则表达式进行排除，等等
				 */
				// 指定新的过滤规则，这个过滤规则是我们自个自定义的，过滤规则就是由我们这个自定义的MyTypeFilter类返回true或者false来代表匹配还是没匹配
				@Filter(type=FilterType.CUSTOM, classes={MyTypeFilter.class})
		}, useDefaultFilters=false)
```

MyTypeFilter是一个自定义且实现了org.springframework.core.type.filter.TypeFilter接口的类，在该类中通过重写match方法进行过滤规则的编写

```java
public class MyTypeFilter implements TypeFilter {

	/**
	 * 参数：
	 * metadataReader：读取到的当前正在扫描的类的信息
	 * metadataReaderFactory：可以获取到其他任何类的信息的（工厂）
	 */
	@Override
	public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory) throws IOException {
		// 获取当前类注解的信息
		AnnotationMetadata annotationMetadata = metadataReader.getAnnotationMetadata();
		// 获取当前正在扫描的类的类信息，比如说它的类型是什么啊，它实现了什么接口啊之类的
		ClassMetadata classMetadata = metadataReader.getClassMetadata();
		// 获取当前类的资源信息，比如说类的路径等信息
		Resource resource = metadataReader.getResource();
		// 获取当前正在扫描的类的类名
		String className = classMetadata.getClassName();
		System.out.println("--->" + className);
		
		// 现在来指定一个规则
		if (className.contains("er")) {
			return true; // 匹配成功，就会被包含在容器中
		}
		
		return false; // 匹配不成功，所有的bean都会被排除
	}

}
```



#### Ⅶ、重复注解

##### [1]、原理

![image-20220713225046496](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713225046496.png)

![image-20220713225154403](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220713225154403.png)

##### [2]、含义

可以在一个类上重复使用这个注解

##### [3]、代码

```java
@ComponentScan(value="com.meimeixia", includeFilters={
		/*
		 * type：指定你要排除的规则，是按照注解进行排除，还是按照给定的类型进行排除，还是按照正则表达式进行排除，等等
		 * classes：我们需要Spring在扫描时，只包含@Controller注解标注的类
		 */
		@Filter(type=FilterType.ANNOTATION, classes={Controller.class})
}, useDefaultFilters=false) 
@ComponentScan(value="com.meimeixia", includeFilters={
		/*
		 * type：指定你要排除的规则，是按照注解进行排除，还是按照给定的类型进行排除，还是按照正则表达式进行排除，等等
		 * classes：我们需要Spring在扫描时，只包含@Service注解标注的类
		 */
		@Filter(type=FilterType.ANNOTATION, classes={Service.class})
}, useDefaultFilters=false) 
```



## 2、组件添加

### ①、@scope

#### Ⅰ、注解概述

##### [1]、作用

@Scope注解能够设置组件的作用域

##### [2]、取值

![在这里插入图片描述](https://ningct.oss-cn-hangzhou.aliyuncs.com/20201128175053847.png)

##### [3]、用法

```Java
@Scope("prototype") // 通过@Scope注解来指定该bean的作用范围，也可以说成是调整作用域
	@Bean("person")
	public Person person() {
		return new Person("美美侠", 25);
	}
```



#### Ⅱ、单实例bean

##### [1]、含义

+ 对象在Spring容器中默认是单实例
+ Spring容器在启动时就会将实例对象加载到Spring容器中
+ 每次从Spring容器中获取实例对象，都是直接将对象返回，而不必再创建新的实例对象

##### [2]、注意事项

单实例bean是整个应用所共享的，所以需要考虑到线程安全问题，之前在玩SpringMVC的时候，SpringMVC中的Controller默认是单例的，有些开发者在Controller中创建了一些变量，那么这些变量实际上就变成共享的了，Controller又可能会被很多线程同时访问，这些线程并发去修改Controller中的共享变量，此时很有可能会出现数据错乱的问题，所以使用的时候需要特别注意。

#### Ⅲ、多实例bean

##### [1]、含义

+ 需要通过@scope注解并取值才是多实例的

  ```java
  	@Scope("prototype") // 通过@Scope注解来指定该bean的作用范围，也可以说成是调整作用域
  	@Bean("person")
  	public Person person() {
  		return new Person("美美侠", 25);
  	}
  ```

  

+ 启动时不会将实例对象创建

+ 每次从IOC获取对象的时候创建新的对象并返回

##### [4]、注意事项

多实例bean每次获取的时候都会重新创建，如果这个bean比较复杂，创建时间比较长，那么就会影响系统的性能，因此这个地方需要注意点。

#### Ⅳ、自定义scope

如果Spring内置的几种scope都无法满足我们的需求时，我们可以自定义bean的作用域

##### [1]、自定义类实现Scope接口

+ 定义一个常量供@scope取值
+ 重写方法
+ 自定义检索规则get()

```java
/**
 * 自定义本地线程级别的bean作用域，不同的线程中的bean是不同的实例，同一个线程中同名的bean是同一个实例
 * @author liayun
 *
 */
public class ThreadScope implements Scope {
	
	public static final String THREAD_SCOPE = "thread";
	
	private ThreadLocal<Map<String, Object>> beanMap = new ThreadLocal() {
		
		@Override
        protected Object initialValue() {
            return new HashMap<>();
        }
		
	};
	
	/**
	 * 返回当前作用域中name对应的bean对象
	 * @param name：需要检索的bean对象的名称
	 * @param objectFactory：如果name对应的bean对象在当前作用域中没有找到，那么可以调用这个objectFactory来创建这个bean对象
	 */
	@Override
	public Object get(String name, ObjectFactory<?> objectFactory) {
		Object bean = beanMap.get().get(name);
		if (Objects.isNull(bean)) {
			bean = objectFactory.getObject();
			beanMap.get().put(name, bean);
		}
		return bean;
	}

	/**
	 * 将name对应的bean对象从当前作用域中移除
	 */
	@Override
	public Object remove(String name) {
		return this.beanMap.get().remove(name);
	}

	/**
	 * 用于注册销毁回调，若想要销毁相应的对象，则由Spring容器注册相应的销毁回调，而由自定义作用域选择是不是要销毁相应的对象
	 */
	// bean作用域范围结束的时候调用的方法，用于bean的清理
	@Override
	public void registerDestructionCallback(String name, Runnable callback) {
		System.out.println(name);
	}

	/**
	 * 用于解析相应的上下文数据，比如request作用域将返回request中的属性
	 */
	@Override
	public Object resolveContextualObject(String key) {
		return null;
	}

	/**
	 * 作用域的会话标识，比如session作用域的会话标识是sessionId
	 */
	@Override
	public String getConversationId() {
		return Thread.currentThread().getName();
	}

}
```



##### [2]、将自定义Scope类注册到容器中

```java
	AnnotationConfigApplicationContext applicationContext = new
   	 						AnnotationConfigApplicationContext(MainConfig3.class);
    // 向容器中注册自定义的Scope
    applicationContext.getBeanFactory().registerScope(ThreadScope.THREAD_SCOPE, new
                                                      ThreadScope());
```



##### [3]、使用自定义的作用域

```java
@Scope("thread")
	@Bean("person")
	public Person person() {
		System.out.println("给容器中添加咱们这个Person对象...");
		return new Person("美美侠", 25);
	}
```



### ②、@Lazy

#### Ⅰ、注解概述

懒加载，也称延时加载，仅针对单实例bean生效。

 单实例bean是在Spring容器启动的时候加载的，添加@Lazy注解后就会延迟加载，在Spring容器启动的时候并不会加载，而是在第一次使用此bean的时候才会加载

#### Ⅱ、用法

```java
	@Lazy
	@Bean("person")
	public Person person() {
		System.out.println("给容器中添加咱们这个Person对象...");
		return new Person("美美侠", 25);
	}
```



### ③、@Conditional

#### Ⅰ、注解概述

+ @Conditional注解可以按照一定的条件进行判断，满足条件向容器中注册bean，不满足条件就不向容器中注册bean。

+ 要想使用@Conditional注解，需要定义实现Condition接口的类来为@Conditional注解设置条件

#### Ⅱ、用法

##### [1]、自定义类实现Condition接口，重写matches方法

```java
/**
* 判断操作系统是否是Linux系统
* @author liayun
*
*/
public class LinuxCondition implements Condition {

    /**
    * ConditionContext：判断条件能使用的上下文（环境）
    * AnnotatedTypeMetadata：当前标注了@Conditional注解的注释信息
    */
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        // 判断操作系统是否是Linux系统
        
        // 1. 获取到bean的创建工厂（能获取到IOC容器使用到的BeanFactory，它就是创建对象以及进行装配的工厂）
        ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
        // 2. 获取到类加载器
        ClassLoader classLoader = context.getClassLoader();
        // 3. 获取当前环境信息，它里面就封装了我们这个当前运行时的一些信息，包括环境变量，以及包括虚拟机的一些变量
        Environment environment = context.getEnvironment();
        // 4. 获取到bean定义的注册类
        BeanDefinitionRegistry registry = context.getRegistry();
        
        String property = environment.getProperty("os.name");
        if (property.contains("linux")) {
            return true;
        }
        
        return false;
    }

}
```

> BeanDefinitionRegistry详解：
>
> Spring容器中所有的bean都可以通过BeanDefinitionRegistry对象来进行注册，因此我们可以向Spring容器中注册一个bean、移除一个bean、查询某一个bean的定义信息或者判断Spring容器中是否包含有某一个bean的定义，据此编写条件逻辑。

##### [2]、在配置类使用注解

###### (1)、方法上使用

 满足当前条件，这个bean注册才能生效

```java
	@Conditional({WindowsCondition.class})
	@Bean("bill")
	public Person person01() {
		return new Person("Bill Gates", 62);
	}
```

###### (2)、类上使用

```java
// 对配置类中的组件进行统一设置
@Conditional({WindowsCondition.class}) // 满足当前条件，这个类中配置的所有bean注册才能生效
@Configuration
public class MainConfig2 {
	
}
```



#### Ⅲ、扩展

Conditional有一系列封装的子类接口供选择

![在这里插入图片描述](https://ningct.oss-cn-hangzhou.aliyuncs.com/20201129173723444.png)

### ④、@Import

#### Ⅰ、注册Bean方式

##### [1]、包扫描+给组件标注注解

@Controller、@Servcie、@Repository、@Component，但这种方式比较有局限性，局限于我们自己写的类

##### [2]、@Bean注解

通常用于导入第三方包中的组件

##### [3]、@Import注解

快速向Spring容器中导入一个组件

#### Ⅱ、注解详解

@Import注解提供了@Bean注解的功能，同时还有XML配置文件里面标签组织多个分散的XML文件的功能，当然在这里是组织多个分散的@Configuration，因为一个配置类就约等于一个XML配置文件。



@Import可以配合`Configuration`、`ImportSelector`以及`ImportBeanDefinitionRegistrar`来使用，也可以把Import当成普通的bean来使用。



@Import注解只允许放到类上面，不允许放到方法上。



#### Ⅲ、用法

##### [1]、直接填写class数组的方式

```java
@Configuration
@Import(Color.class) // @Import快速地导入组件，id默认是组件的全类名
public class MainConfig2 {
	
}
```

```java
@Configuration
@Import({Color.class, Red.class}) // @Import快速地导入组件，id默认是组件的全类名
public class MainConfig2 {
	
}
```



##### [2]、ImportSelector接口的方式

###### (1)、ImportSelector接口概述

ImportSelector接口是Spring中导入外部配置的核心接口，在Spring Boot的自动化配置和@EnableXXX（功能性注解）都有它的存在。



其主要作用是收集需要导入的配置类，selectImports()方法的返回值就是我们向Spring容器中导入的类的全类名。



如果该接口的实现类同时实现EnvironmentAware，BeanFactoryAware，BeanClassLoaderAware或者ResourceLoaderAware，那么在调用其selectImports()方法之前先调用上述接口中对应的方法。



如果需要在所有的@Configuration处理完再导入时，那么可以实现DeferredImportSelector接口。



在ImportSelector接口的selectImports()方法中，存在一个AnnotationMetadata类型的参数，这个参数能够获取到当前标注@Import注解的类的所有注解信息，也就是说不仅能获取到@Import注解里面的信息，还能获取到其他注解的信息。

###### (2)、用法

+ 创建一个MyImportSelector类实现ImportSelector接口

  ```java
  /**
   * 自定义逻辑，返回需要导入的组件
   * @author liayun
   *
   */
  public class MyImportSelector implements ImportSelector {
  
  	// 返回值：就是要导入到容器中的组件的全类名
  	// AnnotationMetadata：当前标注@Import注解的类的所有注解信息，也就是说不仅能获取到@Import注解里面的信息，还能获取到其他注解的信息
  	@Override
  	public String[] selectImports(AnnotationMetadata importingClassMetadata) { // 在这一行打个断点，debug调试一下
  		
  		// 方法不要返回null值，否则会报空指针异常
  //		return new String[]{}; // 可以返回一个空数组
  		return new String[]{"com.meimeixia.bean.Bule",
                              "com.meimeixia.bean.Yellow"};
  	}
  
  }
  ```

  

+ 在MainConfig2配置类的@Import注解中，导入MyImportSelector类

  ```java
  @Import({Color.class, Red.class, MyImportSelector.class}) // @Import快速地导入组件，id默认是组件的全类名
  public class MainConfig2 {
  	
  }
  ```

  

##### [3]、mportBeanDefinitionRegistrar接口方式

###### (1)、mportBeanDefinitionRegistrar接口概述

在ImportBeanDefinitionRegistrar接口中，有一个registerBeanDefinitions()方法，通过该方法，我们可以向Spring容器中注册bean实例。



Spring官方在动态注册bean时，大部分套路其实是使用ImportBeanDefinitionRegistrar接口。



所有实现了该接口的类都会被ConfigurationClassPostProcessor处理，ConfigurationClassPostProcessor实现了BeanFactoryPostProcessor接口，所以ImportBeanDefinitionRegistrar中动态注册的bean是优先于依赖其的bean初始化的，也能被aop、validator等机制处理。



###### (2)、用法

+ 创建一个MyImportBeanDefinitionRegistrar类，去实现ImportBeanDefinitionRegistrar接口

  ```java
  public class MyImportBeanDefinitionRegistrar implements
      ImportBeanDefinitionRegistrar {
  
  	/**
  	 * AnnotationMetadata：当前类的注解信息
  	 * BeanDefinitionRegistry：BeanDefinition注册类
  	 * 
  	 * 我们可以通过调用BeanDefinitionRegistry接口中的registerBeanDefinition方法，手动注册所有需要添加到容器中的bean
  	 */
  	@Override
  	public void registerBeanDefinitions(AnnotationMetadata
                                          importingClassMetadata,
                                          BeanDefinitionRegistry registry) {
  		boolean definition =
              registry.containsBeanDefinition("com.meimeixia.bean.Red");
  		boolean definition2 =
              registry.containsBeanDefinition("com.meimeixia.bean.Bule");
  		if (definition && definition2) {
  			// 指定bean的定义信息，包括bean的类型、作用域等等
  			// RootBeanDefinition是BeanDefinition接口的一个实现类
  			RootBeanDefinition beanDefinition = new
                  RootBeanDefinition(RainBow.class); // bean的定义信息
  			// 注册一个bean，并且指定bean的名称
  			registry.registerBeanDefinition("rainBow", beanDefinition);
  		}
  	}
  }
  ```

  

+ 在MainConfig2配置类上的@Import注解中，添加MyImportBeanDefinitionRegistrar类

  ```java
  @Configuration
  @Import({Color.class, Red.class, MyImportSelector.class, MyImportBeanDefinitionRegistrar.class}) // @Import快速地导入组件，id默认是组件的全类名
  public class MainConfig2 {
  	
  }
  ```

  

### ⑤、Bean控制生命周期

#### Ⅰ、使用@Bean指定初始化和销毁

##### [1]、Bean自定义初始化和销毁方法

```java
public class Car {

	public Car() {
		System.out.println("car constructor...");
	}
	
	public void init() {
		System.out.println("car ... init...");
	}
	
	public void destroy() {
		System.out.println("car ... destroy...");
	}
	
}
```



##### [2]、@Bean指定初始化和销毁方法

```java
@Configuration
public class MainConfigOfLifeCycle {

	@Bean(initMethod="init", destroyMethod="destroy")
	public Car car() {
		return new Car();
	}

}
```

> bean的销毁方法是在容器关闭的时候被调用的

#### Ⅱ、实现InitializingBean接口和DisposableBean接口实现初始化和销毁

##### [1]、InitializingBean接口详解

Spring中提供了一个InitializingBean接口，该接口为bean提供了属性初始化后的处理方法，它只包括afterPropertiesSet方法，凡是继承该接口的类，在bean的属性初始化后都会执行该方法。

##### [2]、DisposableBean接口详解

实现org.springframework.beans.factory.DisposableBean接口的bean在销毁前，Spring将会调用DisposableBean接口的destroy()方法。也就是说我们可以实现DisposableBean这个接口来定义咱们这个销毁的逻辑。

##### [3]、用法

```java
@Component
public class Cat implements InitializingBean, DisposableBean {
	
	public Cat() {
		System.out.println("cat constructor...");
	}

	/**
	 * 会在容器关闭的时候进行调用
	 */
	@Override
	public void destroy() throws Exception {
		// TODO Auto-generated method stub
		System.out.println("cat destroy...");
	}

	/**
	 * 会在bean创建完成，并且属性都赋好值以后进行调用
	 */
	@Override
	public void afterPropertiesSet() throws Exception {
		// TODO Auto-generated method stub
		System.out.println("cat afterPropertiesSet...");
	}

}
```

##### [4]、注意事项

多实例bean的生命周期不归Spring容器来管理，这里的DisposableBean接口中的方法是由Spring容器来调用的，所以如果一个多实例bean实现了DisposableBean接口是没有啥意义的，因为相应的方法根本不会被调用，当然了，在XML配置文件中指定了destroy方法，也是没有任何意义的。所以，在多实例bean情况下，Spring是不会自动调用bean的销毁方法的。



#### Ⅲ、使用@PostConstruct注解和@PreDestroy注解指定初始化之前和销毁之后

##### [1]、注解详解

Java自己的注解，是JSR-250规范里面定义的一个注解。



@PostConstruct注解被用来修饰一个非静态的void()方法。

被@PostConstruct注解修饰的方法会在服务器加载Servlet的时候运行，并且只会被服务器执行一次。

被@PostConstruct注解修饰的方法通常在构造函数之后，init()方法之前执行：

> Constructor（构造方法）→@Autowired（依赖注入）→@PostConstruct（注释的方法）

被@PreDestroy注解修饰的方法会在服务器卸载Servlet的时候运行，并且只会被服务器调用一次，类似于Servlet的destroy()方法。

被@PreDestroy注解修饰的方法会在destroy()方法之后，Servlet被彻底卸载之前执行：

> 调用destroy()方法→@PreDestroy→destroy()方法→bean销毁

##### [2]、用法

```java
@Component
public class Dog {

	public Dog() {
		System.out.println("dog constructor...");
	}
	
	// 在对象创建完成并且属性赋值完成之后调用
	@PostConstruct
	public void init() {
		System.out.println("dog...@PostConstruct...");
	}
	
	// 在容器销毁（移除）对象之前调用
	@PreDestroy
	public void destory() {
		System.out.println("dog...@PreDestroy...");
	}
	
}
```



#### Ⅳ、初始化和销毁使用场景

一个典型的使用场景就是对于数据源的管理。例如，在配置数据源时，在初始化的时候，会对很多的数据源的属性进行赋值操作；在销毁的时候，我们需要对数据源的连接等信息进行关闭和清理。这个时候，我们就可以在自定义的初始化和销毁方法中来做这些事情了。

#### Ⅴ、初始化和销毁时机

+ bean对象的初始化方法调用的时机：对象创建完成，如果对象中存在一些属性，并且这些属性也都赋好值之后，那么就会调用bean的初始化方法。对于单实例bean来说，在Spring容器创建完成后，Spring容器会自动调用bean的初始化方法；对于多实例bean来说，在每次获取bean对象的时候，调用bean的初始化方法。

+ bean对象的销毁方法调用的时机：对于单实例bean来说，在容器关闭的时候，会调用bean的销毁方法；对于多实例bean来说，Spring容器不会管理这个bean，也就不会自动调用这个bean的销毁方法了。

  

#### Ⅵ、BeanPostProcessor初始化前后

##### [1]、概述

BeanPostProcessor是一个接口，其中有两个方法，即postProcessBeforeInitialization和postProcessAfterInitialization这两个方法，这两个方法分别是在Spring容器中的bean初始化前后执行，所以Spring容器中的每一个bean对象初始化前后，都会执行BeanPostProcessor接口的实现类中的这两个方法。



当容器中存在多个BeanPostProcessor的实现类时，会按照它们在容器中注册的顺序执行。对于自定义的BeanPostProcessor实现类，还可以让其实现Ordered接口自定义排序。



##### [2]、自定义后置处理器

```java
@Component // 将后置处理器加入到容器中，这样的话，Spring就能让它工作了
public class MyBeanPostProcessor implements BeanPostProcessor {

	@Override
	public Object postProcessBeforeInitialization(Object bean, String
                                                  beanName) throws
        BeansException {
		// TODO Auto-generated method stub
		System.out.println("postProcessBeforeInitialization..." + beanName +
                           "=>" + bean);
		return bean;
	}

	@Override
	public Object postProcessAfterInitialization(Object bean, String
                                                 beanName) throws
        BeansException {
		// TODO Auto-generated method stub
		System.out.println("postProcessAfterInitialization..." + beanName +
                           "=>" + bean);
		return bean;
	}

}
```

自定义排序

```java
@Component // 将后置处理器加入到容器中，这样的话，Spring就能让它工作了
public class MyBeanPostProcessor implements BeanPostProcessor, Ordered {

	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		// TODO Auto-generated method stub
		System.out.println("postProcessBeforeInitialization..." + beanName + "=>" + bean);
		return bean;
	}

	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		// TODO Auto-generated method stub
		System.out.println("postProcessAfterInitialization..." + beanName + "=>" + bean);
		return bean;
	}

	@Override
	public int getOrder() {
		// TODO Auto-generated method stub
		return 3;
	}

}
```



##### [3]、作用

后置处理器可用于bean对象初始化前后进行逻辑增强。

Spring提供了BeanPostProcessor接口的很多实现类，例如AutowiredAnnotationBeanPostProcessor用于@Autowired注解的实现，AnnotationAwareAspectJAutoProxyCreator用于Spring AOP的动态代理等等。



##### [4]、执行流程

创建了一个IOC容器



AnnotationConfigApplicationContext类的构造方法中会调用refresh()方法



refresh()方法里面调用了finishBeanFactoryInitialization()方法，初始化所有的（非懒加载的）单实例bean对象



DefaultListableBeanFactory类的preInstantiateSingletons()方法的最后一个else分支调用的getBean()方法



在getBean()方法中，又调用了doGetBean()方法



通过getSingleton()方法来获取单实例bean



在getSingleton()方法里面又调用了getObject()方法来获取单实例bean



第一次获取单实例bean时，由于单实例bean还未创建，那么Spring会调用createBean()方法来创建单实例bean



AbstractAutowireCapableBeanFactory类的createBean()方法



调用的是doCreateBean()方法



AbstractAutowireCapableBeanFactory类的initializeBean()方法



applyBeanPostProcessorsBeforeInitialization()方法



populateBean()方法，为bean的属性赋好值



在initializeBean()方法中，调用了invokeInitMethods()方法，invokeInitMethods()方法的作用就是执行初始化方法。

在调用invokeInitMethods()方法之前，Spring调用了applyBeanPostProcessorsBeforeInitialization()这个方法；

在调用invokeInitMethods()方法之后，Spring又调用了applyBeanPostProcessorsAfterInitialization()这个方法；

在applyBeanPostProcessorsBeforeInitialization()方法中，会遍历所有BeanPostProcessor对象，然后依次执行所有BeanPostProcessor对象的postProcessBeforeInitialization()方法，一旦BeanPostProcessor对象的postProcessBeforeInitialization()方法返回null以后，则后面的BeanPostProcessor对象便不再执行了，而是直接退出for循环；



也就是说，在Spring中，调用initializeBean()方法之前，调用了populateBean()方法为bean的属性赋值，为bean的属性赋好值之后，再调用initializeBean()方法进行初始化。

在initializeBean()中，调用自定义的初始化方法（即invokeInitMethods()）之前，调用了applyBeanPostProcessorsBeforeInitialization()方法，而在调用自定义的初始化方法之后，又调用了applyBeanPostProcessorsAfterInitialization()方法。至此，整个bean的初始化过程就这样结束了。

##### [5]、底层的使用

###### (1)、ApplicationContextAwareProcessor类

org.springframework.context.support.ApplicationContextAwareProcessor是BeanPostProcessor接口的一个实现类，这个类的作用是可以向组件中注入IOC容器



如果需要向组件中注入IOC容器，那么可以让组件实现ApplicationContextAware接口



实现ApplicationContextAware接口中的setApplicationContext()方法，在setApplicationContext()方法中有一个ApplicationContext类型的参数，这个就是IOC容器对象

```java
@Component
public class Dog implements ApplicationContextAware {
	
	private ApplicationContext applicationContext;

	public Dog() {
		System.out.println("dog constructor...");
	}
	
	// 在对象创建完成并且属性赋值完成之后调用
	@PostConstruct
	public void init() { 
		System.out.println("dog...@PostConstruct...");
	}
	
	// 在容器销毁（移除）对象之前调用
	@PreDestroy
	public void destory() {
		System.out.println("dog...@PreDestroy...");
	}

	@Override
	public void setApplicationContext(ApplicationContext applicationContext) throws BeansException { 
		// TODO Auto-generated method stub
		this.applicationContext = applicationContext;
	}
	
}
```



在ApplicationContextAwareProcessor的postProcessBeforeInitialization()方法中，在bean初始化之前，首先对当前bean的类型进行判断，如果当前bean的类型不是EnvironmentAware，不是EmbeddedValueResolverAware，不是ResourceLoaderAware，不是ApplicationEventPublisherAware，不是MessageSourceAware，也不是ApplicationContextAware，那么直接返回bean。如果是上面类型中的一种类型，那么最终会调用invokeAwareInterfaces()方法，并将bean传递给该方法。



invokeAwareInterfaces()方法的源码比较简单，就是判断当前bean属于哪种接口类型，然后将bean强转为哪种接口类型的对象，接着调用接口中的方法，将相应的参数传递到接口的方法中

此时会将this.applicationContext传递到ApplicationContextAware接口的setApplicationContext()方法中



###### (2)、BeanValidationPostProcessor类

org.springframework.validation.beanvalidation.BeanValidationPostProcessor类主要是用来为bean进行校验操作的，当我们创建bean，并为bean赋值后，我们可以通过BeanValidationPostProcessor类为bean进行校验操作。



在BeanValidationPostProcessor类的postProcessBeforeInitialization()方法和postProcessAfterInitialization()方法中的主要逻辑都是调用doValidate()方法对bean进行校验，只不过在这两个方法中都会对afterInitialization这个boolean类型的成员变量进行判断，若afterInitialization的值为false，则在postProcessBeforeInitialization()方法中调用doValidate()方法对bean进行校验；若afterInitialization的值为true，则在postProcessAfterInitialization()方法中调用doValidate()方法对bean进行校验。


###### (3)、InitDestroyAnnotationBeanPostProcessor类

org.springframework.beans.factory.annotation.InitDestroyAnnotationBeanPostProcessor类主要用来处理@PostConstruct注解和@PreDestroy注解。



Spring怎么就知道什么时候执行@PostConstruct注解标注的方法，什么时候执行@PreDestroy注解标注的方法呢？这就要归功于InitDestroyAnnotationBeanPostProcessor类了。



在InitDestroyAnnotationBeanPostProcessor类的postProcessBeforeInitialization()方法中，首先会找到bean中有关生命周期的注解，比如@PostConstruct注解等，找到这些注解之后，则将这些信息赋值给LifecycleMetadata类型的变量metadata，之后调用metadata的invokeInitMethods()方法，通过反射来调用标注了@PostConstruct注解的方法。这就是为什么标注了@PostConstruct注解的方法会被Spring执行的原因。



###### (4)、AutowiredAnnotationBeanPostProcessor类

org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor类主要是用于处理标注了@Autowired注解的变量或方法。





## 3、组件赋值

### ①、@Value

#### Ⅰ、详解

@Value注解可以为bean中的属性赋值



@Value注解可以标注在字段、方法、参数以及注解上，而且在程序运行期间生效



#### Ⅱ、不通过配置文件注入

##### [1]、普通字符串

```java
@Value("李阿昀")
private String name; // 注入普通字符串

```



##### [2]、操作系统属性

```java
@Value("#{systemProperties['os.name']}")
private String systemPropertiesName; // 注入操作系统属性

```



##### [3]、SpEL表达式结果

```java
@Value("#{ T(java.lang.Math).random() * 100.0 }")
private double randomNumber; //注入SpEL表达式结果

```



##### [4]、其他bean中属性的值

```java
@Value("#{person.name}")
private String username; // 注入其他bean中属性的值，即注入person对象的name属性中的值

```



##### [5]、文件资源

```java
@Value("classpath:/config.properties")
private Resource resourceFile; // 注入文件资源

```



##### [6]、URL资源

```java
@Value("http://www.baidu.com")
private Resource url; // 注入URL资源

```



#### Ⅲ、通过配置文件注入属性

##### [1]、新建属性文件

在项目的src/main/resources目录下新建一个属性文件，例如person.properties

```properties
person.nickName=美美侠

```



##### [2]、使用@PropertySource注解读取外部配置文件

读取外部配置文件中的key/value并保存到运行的环境变量中

```java
@PropertySource(value={"classpath:/person.properties"})
@Configuration
public class MainConfigOfPropertyValues {

	@Bean
	public Person person() {
		return new Person();
	}
	
}

```



##### [3]、使用`${key}`取出

使用${key}取出配置文件中key所对应的值，并将其注入到bean的属性中了

```java
	@Value("李阿昀")
	private String name;
	@Value("#{20-2}")
	private Integer age;
	
	@Value("${person.nickName}")
	private String nickName; // 昵称

```



#### Ⅳ、#{···}和${···}的区别

- `#{···}`：用于执行SpEl表达式，并将内容赋值给属性

- `${···}`：主要用于加载外部属性文件中的值

- `${···}`和`#{···}`可以混合使用，但是必须`#{}`在外面，`${}`在里面

  > Spring执行`${}`的时机要早于`#{}`

### ②、@PropertySource

#### Ⅰ、注解概述

通过@PropertySource注解可以将properties配置文件中的key/value存储到Spring的Environment中，Environment接口提供了方法去读取配置文件中的值，参数是properties配置文件中定义的key值。



也可以使用@Value注解用${}占位符为bean的属性注入值。


#### Ⅱ、多个properties文件

```java
@PropertySource(value={"classpath:/person.properties", "classpath:/car.properties"})

```



#### Ⅲ、@PropertySources

@PropertySources注解的源码比较简单，只有一个PropertySource[]数组类型的value属性

```java
@PropertySources(value={
    @PropertySource(value={"classpath:/person.properties"}),
    @PropertySource(value={"classpath:/car.properties"}),
})

```



#### Ⅳ、Environment

使用@PropertySource注解读取外部配置文件中的key/value之后，是将其保存到运行的环境变量中了，所以我们也可以通过运行环境来获取外部配置文件中的值

```java
	ConfigurableEnvironment environment =
	applicationContext.getEnvironment();
    String property = environment.getProperty("person.nickName");
    System.out.println(property);
```



### ③、@Autowired

#### Ⅰ、@Autowired

##### [1]、概述

@Autowired注解可以对类成员变量、方法和构造函数进行标注，完成自动装配的工作。

@Autowired注解可以放在类、接口以及方法上。

##### [2]、执行

@Autowired注解默认是优先按照类型去容器中找对应的组件，相当于是调用了如下这个方法：

```java
applicationContext.getBean(类名.class);
若找到则就赋值。
```

如果找到多个相同类型的组件，那么是将属性名称作为组件的id，到IOC容器中进行查找，这时就相当于是调用了如下这个方法：

```java
applicationContext.getBean("组件的id");
```

##### [3]、找不到Bean

当为@Autowired注解添加属性required=false后，即使IOC容器中没有对应的对象，Spring也不会抛出异常了。不过，此时装配的对象就为null

```java
	@Qualifier("bookDao")
	@Autowired(required=false)
	private BookDao bookDao2;
```



#### Ⅱ、@Qualifier

##### [1]、概述

@Autowired是根据类型进行自动装配的，如果需要按名称进行装配，那么就需要配合@Qualifier注解来使用了。

##### [2]、用法

```java
	@Qualifier("bookDao")
	@Autowired
	private BookDao bookDao2;
```

> 尽管字段的名称为bookDao2，但是我们使用了@Qualifier注解显示指定了@Autowired注解装配bookDao对象，所以，装配了bookDao对象的信息

#### Ⅲ、@Primary

##### [1]、概述

在Spring中使用注解时，常常会使用到@Autowired这个注解，它默认是根据类型Type来自动注入的。但有些特殊情况，对同一个接口而言，可能会有几种不同的实现类，而在默认只会采取其中一种实现的情况下，就可以使用@Primary注解来标注优先使用哪一个实现类。



如果是在没有明确指定的情况下，那么就装配优先级最高的首选的那个bean，如果是在明确指定了的情况下，那么自然就是装配指定的那个bean



##### [2]、用法

```java
	@Primary
	@Bean("bookDao2")
	public BookDao bookDao() {
		BookDao bookDao = new BookDao();
		bookDao.setLable("2");
		return bookDao;
	}
```

> 需要注释掉BookService类中bookDao字段上的@Qualifier注解，这是因为@Qualifier注解为显示指定装配哪个组件，如果使用了@Qualifier注解，无论是否使用了@Primary注解，都会装配@Qualifier注解标注的对象。

#### Ⅳ、@Resource和@Inject

##### [1]、概述

###### (1)、@Resource

@Resource注解是Java规范里面的，也可以说它是JSR250规范里面定义的一个注解。

该注解默认按照名称进行装配，名称可以通过name属性进行指定，如果没有指定name属性，当注解写在字段上时，那么默认取字段名将其作为组件的名称在IOC容器中进行查找，如果注解写在setter方法上，那么默认取属性名进行装配。

当找不到与名称匹配的bean时才按照类型进行装配。

但是需要注意的一点是，如果name属性一旦指定，那么就只会按照名称进行装配。

###### (2)、@Inject

@Inject注解也是Java规范里面的，也可以说它是JSR330规范里面定义的一个注解。

该注解默认是根据参数名去寻找bean注入，支持Spring的@Primary注解优先注入，@Inject注解还可以增加@Named注解指定要注入的bean。

> 想使用@Inject注解，需要在项目的pom.xml文件中添加如下依赖，即导入javax.inject这个包

##### [2]、用法

###### (1)、@Resource

@Resource注解和@Autowired注解的功能是一样的，都能实现自动装配，只不过@Resource注解默认是按照组件名称（即属性的名称）进行装配的。

虽然@Resource注解具备自动装配这一功能，但是它是不支持@Primary注解优先注入的功能的，而且也不能像@Autowired注解一样能添加required=false属性。



可以通过@Resource注解的name属性显示指定要装配的组件的名称

```java
	@Resource(name="bookDao2")
	private BookDao bookDao;
```

###### (2)、@Inject

@Inject注解和@Autowired注解的功能是一样的，都能实现自动装配，而且它俩都支持@Primary注解优先注入的功能。

只不过，@Inject注解不能像@Autowired注解一样能添加required=false属性，因为它里面没啥属性。

##### [3]、与@Autowired注解的区别

+ @Autowired是Spring中的专有注解，而@Resource是Java中JSR250规范里面定义的一个注解，@Inject是Java中JSR330规范里面定义的一个注解
+ @Autowired支持参数required=false，而@Resource和@Inject都不支持
+ @Autowired和@Inject支持@Primary注解优先注入，而@Resource不支持
+ @Autowired通过@Qualifier指定注入特定bean，@Resource可以通过参数name指定注入bean，而@Inject需要通过@Named注解指定注入bean

#### Ⅴ、@Autowired深入

##### [1]、标注在实例方法上

当@Autowired注解标注在方法上时，Spring容器在创建当前对象的时候，就会调用相应的方法为对象赋值。

如果标注的方法存在参数时，那么方法使用的参数和自定义类型的值，需要从IOC容器中获取。

```java
@Autowired 
public void setCar(Car car) {
    this.car = car;
}

```



##### [2]、标注在构造方法上

使用@Autowired注解标注在构造方法上时，构造方法中的参数对象也是从IOC容器中获取的。

> 使用@Autowired注解标注在构造方法上时，如果组件中只有一个有参构造方法，那么这个有参构造方法上的@Autowired注解可以省略，并且参数位置的组件还是可以自动从IOC容器中获取。

```java
	@Autowired
	public Boss(Car car) {
		this.car = car;
		System.out.println("Boss...有参构造器");
	}
```



##### [3]、标注在参数上

无论@Autowired注解是标注在字段上、实例方法上、构造方法上还是参数上，参数位置的组件都是从IOC容器中获取。

> 如果Spring的bean中只有一个有参构造方法，并且这个有参构造方法只有一个参数，这个参数还是IOC容器中的对象，当@Autowired注解标注在这个构造方法的参数上时，那么我们可以将其省略掉

```java
public Boss(@Autowired Car car) {
    this.car = car;
    System.out.println("Boss...有参构造器");
}

```



##### [4]、标注在方法位置

@Autowired注解可以标注在某个方法的位置上

```java
@Bean
public Color color(@Autowired Car car) {
    Color color = new Color();
    color.setCar(car);
    return color;
}

```

也可以省略@Autowired

```java
@Bean
public Color color() {
    Color color = new Color();
    return color;
}

```

> 如果方法只有一个IOC容器中的对象作为参数，当@Autowired注解标注在这个方法的参数上时，我们可以将@Autowired注解省略掉。
>
> 也就说@Bean注解标注的方法在创建对象的时候，方法参数的值是从IOC容器中获取的，此外，标注在这个方法的参数上的@Autowired注解可以省略。

##### [5]、

### ④、XxxAware接口

#### Ⅰ、概述

自定义的组件要想使用Spring容器底层的一些组件，比如ApplicationContext（IOC容器）、底层的BeanFactory等等，那么只需要让自定义组件实现XxxAware接口即可。

此时，Spring在创建对象的时候，会调用XxxAware接口中定义的方法注入相关的组件。



实现ApplicationContextAware接口的话，需要实现setApplicationContext()方法。

在IOC容器启动并创建Dog对象时，Spring会调用setApplicationContext()方法，并且会将ApplicationContext对象传入到setApplicationContext()方法中，我们只需要在Dog类中定义一个ApplicationContext类型的成员变量来接收setApplicationContext()方法中的参数，那么便可以在Dog类的其他方法中使用ApplicationContext对象了。



#### Ⅱ、原理

XxxAware接口的底层原理是由XxxAwareProcessor实现类实现的，也就是说每一个XxxAware接口都有它自己对应的XxxAwareProcessor实现类



ApplicationContextAwareProcessor类的postProcessBeforeInitialization()方法调用invokeAwareInterfaces()方法，根据实现的接口进行强制类型转换匹配相应的组件进行注入



### ⑤、@Profile

#### Ⅰ、概述

@Profile注解是Spring为我们提供的可以根据当前环境，动态地激活和切换一系列组件的功能。

这个功能在不同的环境使用不同的变量的情景下特别有用，例如，开发环境、测试环境、生产环境使用不同的数据源，在不改变代码的情况下，可以使用这个注解来动态地切换要连接的数据库。



#### Ⅱ、作用

+ @Profile注解不仅可以标注在方法上，也可以标注在配置类上。
+ 如果@Profile注解标注在配置类上，那么只有是在指定的环境的时候，整个配置类里面的所有配置才会生效。
+ 如果一个bean上没有使用@Profile注解进行标注，那么这个bean在任何环境下都会被注册到IOC容器中，当然了，前提是在整个配置类生效的情况下。
+ 通过@Profile注解加了环境标识的bean，只有这个环境被激活的时候，相应的bean才会被注册到IOC容器中

#### Ⅲ、用法

```java
	@Profile("test")
	@Bean("testDataSource")
	public DataSource dataSourceTest(@Value("${db.password}") String pwd) throws Exception {
		ComboPooledDataSource dataSource = new ComboPooledDataSource();
		dataSource.setUser(user);
		dataSource.setPassword(pwd);
		dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/test");
		dataSource.setDriverClass(dirverClass);
		return dataSource;
	}
	 
	@Profile("dev") // 定义了一个环境标识，只有当dev环境被激活以后，我们这个bean才能被注册进来
	@Bean("devDataSource")
	public DataSource dataSourceDev(@Value("${db.password}") String pwd) throws Exception {
		ComboPooledDataSource dataSource = new ComboPooledDataSource();
		dataSource.setUser(user);
		dataSource.setPassword(pwd);
		dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/ssm_crud");
		dataSource.setDriverClass(dirverClass);
		return dataSource;
	}
	
	@Profile("prod")
	@Bean("prodDataSource")
	public DataSource dataSourceProd(@Value("${db.password}") String pwd) throws Exception {
		ComboPooledDataSource dataSource = new ComboPooledDataSource();
		dataSource.setUser(user);
		dataSource.setPassword(pwd);
		dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/scw_0515");
		dataSource.setDriverClass(dirverClass);
		return dataSource;
	}
```



#### Ⅳ、@Profile("default")

通过@Profile("default")注解来标识一个默认的环境

```java
@Profile("default")
@Bean("devDataSource")
public DataSource dataSourceDev(@Value("${db.password}") String pwd) throws Exception {
    ComboPooledDataSource dataSource = new ComboPooledDataSource();
    dataSource.setUser(user);
    dataSource.setPassword(pwd);
    dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/ssm_crud");
    dataSource.setDriverClass(dirverClass);
    return dataSource;
}
```



## 4、AOP

### ①、概述

AOP是指在程序的运行期间动态地将某段代码切入到指定方法、指定位置进行运行的编程方式。

AOP的底层是使用动态代理实现的。

### ②、使用

#### Ⅰ、导入依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>4.3.12.RELEASE</version>
</dependency>

```



#### Ⅱ、定义目标类

```java
public class MathCalculator {

	public int div(int i, int j) {
		System.out.println("MathCalculator...div...");
		return i / j;
	}
	
}
```



#### Ⅲ、定义切面类

##### [1]、抽取切入点

```java
// 如果切入点表达式都一样的情况下，那么我们可以抽取出一个公共的切入点表达式
	@Pointcut("execution(public int com.meimeixia.aop.MathCalculator.*(..))")
	public void pointCut() {}
```



##### [2]、编写通知

```java
		// @Before：在目标方法（即div方法）运行之前切入
        // @Before("public int com.meimeixia.aop.MathCalculator.*(..)")
        @Before("pointCut()")
        public void logStart() {
            System.out.println("除法运行......@Before，参数列表是：{}");
        }
```

```java
	// 在目标方法（即div方法）结束时被调用
	// @After("pointCut()")
	//在切入点定义以外的类引用切入点
	@After("com.meimeixia.aop.LogAspects.pointCut()")
	public void logEnd() {
		System.out.println("除法结束......@After");
	}
```



#### Ⅳ、加入IOC

```java
@Configuration
public class MainConfigOfAOP {
	
	// 将业务逻辑类（目标方法所在类）加入到容器中
	@Bean
	public MathCalculator calculator() {
		return new MathCalculator();
	}
	
	// 将切面类加入到容器中
	@Bean
	public LogAspects logAspects() {
		return new LogAspects();
	}

}
```



#### Ⅴ、开启AOP

```java
@EnableAspectJAutoProxy//开启AOP
@Configuration
public class MainConfigOfAOP {
}
```



#### Ⅵ、获取目标方法参数

##### [1]、获取方法参数

```java
	@Before("pointCut()")
	public void logStart(JoinPoint joinPoint) {
		// System.out.println("除法运行......@Before，参数列表是：{}");
		
		Object[] args = joinPoint.getArgs(); // 拿到参数列表，即目标方法运行需要的参数列表
		System.out.println(joinPoint.getSignature().getName() + "运行......@Before，参数列表是：{" + Arrays.asList(args) + "}");
	}
```



##### [2]、获取返回值

```java
	@AfterReturning(value="pointCut()", returning="result") 
	// returning来指定我们这个方法的参数谁来封装返回值
	/*
	 * 如果方法正常返回，我们还想拿返回值，那么返回值又应该怎么拿呢？
	 */
	public void logReturn(JoinPoint joinPoint, Object result) { 
		// System.out.println("除法正常返回......@AfterReturning，运行结果是：{}");
		
		System.out.println(joinPoint.getSignature().getName() + "正常返回......@AfterReturning，运行结果是：{" + result + "}");
	}
```



##### [3]、获取异常

```java
// 在目标方法（即div方法）出现异常，被调用
// @AfterThrowing("pointCut()")
@AfterThrowing(value="pointCut()", throwing="exception")
public void logException(JoinPoint joinPoint, Exception exception) {
    // System.out.println("除法出现异常......异常信息：{}");
    
    System.out.println(joinPoint.getSignature().getName() + "出现异常......异常信息：{" + exception + "}");
}
```

> 注意：JoinPoint这个参数要写，一定不能写到后面，它必须出现在参数列表的第一位，否则Spring也是无法识别的，就会报错

### ③、@EnableAspectJAutoProxy

##### [1]、概述

要使注解版的AOP功能起作用的话，那么就得需要在配置类上添加@EnableAspectJAutoProxy注解。

##### [2]、原理

@EnableAspectJAutoProxy注解使用@Import注解给容器中引入了AspectJAutoProxyRegister组件

AspectJAutoProxyRegistrar类实现了ImportBeanDefinitionRegistrar接口通过

ImportBeanDefinitionRegistrar接口实现将自定义的组件添加到IOC容器中

向IOC容器中注册AnnotationAwareAspectJAutoProxyCreator



###### (1)、AnnotationAwareAspectJAutoProxyCreator

此类实现了Aware与BeanPostProcessor接口

BeanPostProcessor：后置处理器，即在bean初始化完成前后做些事情

BeanFactoryAware：自动注入BeanFactory



###### (2)、AnnotationAwareAspectJAutoProxyCreator创建过程：

+ 创建IOC容器AnnotationConfigApplicationContext

  + 首先使用无参构造器创建对象

  + 再来把主配置类注册进来

  + 最后调用refresh()方法刷新容器，刷新容器就是要把容器中的所有bean都创建出来

    + 创建后置处理器

      ```java
      // Register bean processors that intercept bean creation.
      registerBeanPostProcessors(beanFactory);
      ```

      + 先按照类型拿到IOC容器中所有需要创建的后置处理器，即先获取IOC容器中已经定义了的需要创建对象的所有BeanPostProcessor

        > 为什么IOC容器中会有一些已定义的BeanPostProcessor呢？这是因为在前面创建IOC容器时，需要先传入配置类，而我们在解析配置类的时候，由于这个配置类里面有一个@EnableAspectJAutoProxy注解，对于该注解，我们之前也说过，它会为我们容器中注册一个AnnotationAwareAspectJAutoProxyCreator（后置处理器），这还仅仅是这个@EnableAspectJAutoProxy注解做的事，除此之外，容器中还有一些默认的后置处理器的定义。

      + 给容器中加别的BeanPostProcessor
      + 将这些BeanPostProcessor做了一下划分，如果BeanPostProcessor实现了PriorityOrdered接口，那么就将其保存在名为priorityOrderedPostProcessors的List集合中，并且要是该BeanPostProcessor还是MergedBeanDefinitionPostProcessor这种类型的，则还得将其保存在名为internalPostProcessors的List集合中
      + 获取相应名字的BeanPostProcessor，即internalAutoProxyCreator的组件（其实它就是我们之前经常讲的AnnotationAwareAspectJAutoProxyCreator）
      + 从beanFactory中来获取
  
+ AbstractBeanFactory抽象类的getBean()->AbstractBeanFactory抽象类的doGetBean()->DefaultSingletonBeanRegistry类的getSingleton()->singletonFactory的getObject()->AbstractBeanFactory抽象类的doGetBean()->AbstractAutowireCapableBeanFactory抽象类的createBean()->AbstractAutowireCapableBeanFactory抽象类的doCreateBean()->createBeanInstance()
        
      + 给bean的各种属性赋值（即调用populateBean()方法）
      + 始化bean（即调用initializeBean()方法）
        + 处理Aware接口的方法回调
        + 应用后置处理器的postProcessBeforeInitialization()方法
        + invokeInitMethods的方法，即执行自定义的初始化方法
          + 执行Aware接口的方法
            + BeanFactoryAware
              + setBeanFactory()
              + initBeanFactory()方法就是用来初始化BeanFactory
                + AnnotationAwareAspectJAutoProxyCreator这个类的initBeanFactory()方法
                  + 创建了两个东西，一个叫ReflectiveAspectJAdvisorFactory，还有一个叫BeanFactoryAspectJAdvisorsBuilderAdapter
            + 拿到所有的BeanPostProcessor，然后调用beanFactory的addBeanPostProcessor()方法将BeanPostProcessor注册到BeanFactory中
        + 应用后置处理器的postProcessAfterInitialization()方法

###### (3)、BeanFactory的初始化

创建IOC容器AnnotationConfigApplicationContext

+ 首先使用无参构造器创建对象

+ 再来把主配置类注册进来

+ 最后调用refresh()方法刷新容器，刷新容器就是要把容器中的所有bean都创建出来
  + 创建后置处理器
  
  + finishBeanFactoryInitialization()方法处，以完成BeanFactory的初始化工作
  
    > BeanFactory的初始化工作，其实就是来创建剩下的单实例bean。为什么叫剩下的呢？因为IOC容器中的这些组件，比如一些BeanPostProcessor，早都已经在注册的时候就被创建了，所以会留一下没被创建的组件，让它们在这儿进行创建
  
    + 调用preInstantiateSingletons()方法来创建剩下的单实例bean
  
    + 遍历获取容器中所有的bean，并依次创建对象，注意是依次调用getBean()方法来创建对象
  
      + 在doGetBean()中，先从缓存中获取当前bean，如果能获取到，说明当前bean之前是被创建过的，那么就直接使用，否则的话再创建。Spring就是利用这个机制来保证我们这些单实例bean只会被创建一次，也就是说只要创建好的bean都会被缓存起来。
  
      + 调用createBean()方法来进行创建单实例bean
  
        + resolveBeforeInstantiation()方法，给后置处理器一个机会，来返回一个代理对象
  
          + 调用两个方法，一个叫方法叫applyBeanPostProcessorsBeforeInstantiation，另一个方法叫applyBeanPostProcessorsAfterInitialization。
  
            + 拿到所有的后置处理器，如果后置处理器是InstantiationAwareBeanPostProcessor这种类型的，那么就执行该后置处理器的postProcessBeforeInstantiation()方法
  
              具体内容见：[跳转至postProcessBeforeInstantiation](#(4)、postProcessBeforeInstantiation)
  
              > AnnotationAwareAspectJAutoProxyCreator虽然是一个BeanPostProcessor，但是它却是InstantiationAwareBeanPostProcessor这种类型的，而InstantiationAwareBeanPostProcessor接口中声明的方法就叫postProcessBeforeInstantiation。
  
              > AnnotationAwareAspectJAutoProxyCreator会在任何bean创建之前，先尝试返回bean的实例，AnnotationAwareAspectJAutoProxyCreator在所有bean创建之前，会有一个拦截，因为它是InstantiationAwareBeanPostProcessor这种类型的后置处理器，然后会调用它的postProcessBeforeInstantiation()方法
  
        + 调用doCreateBean()方法真正的去创建一个bean实例
  
          + 首先创建bean的实例
          + 然后给bean的各种属性赋值
          + 接着初始化bean
            + 先执行Aware接口的方法
            + 应用后置处理器的postProcessBeforeInitialization()方法
            + 执行自定义的初始化方法
            + 应用后置处理器的postProcessAfterInitialization()方法
              

###### (4)、postProcessBeforeInstantiation

循环创建Bean

每次创建bean的时候，都会先调用postProcessBeforeInstantiation()方法，然后再调用postProcessAfterInitialization()方法



postProcessBeforeInstantiation()：

+ 先来判断当前bean是否在advisedBeans中

  > advisedBeans是个什么东西呢？它是一个Map集合，里面保存了所有需要增强的bean的名称

+ 判断当前bean是否是基础类型，或者是否是切面（标注了@Aspect注解的）

  > 什么叫基础类型呢？所谓的基础类型就是当前bean是否是实现了Advice、Pointcut、Advisor以及AopInfrastructureBean这些接口

  >  用hasAspectAnnotation()方法来判断当前bean有没有标注@Aspect注解

+ 判断是否需要跳过

  + 调用findCandidateAdvisors()方法找到候选的增强器的集合

    > 增强器就是切面里面的那些通知方法
    >
    > 通知方法的详细信息包装成了一个Advisor，并将其存放在了一个List<Advisor>集合中
    >
    > 每一个封装通知方法的增强器都是InstantiationModelAwarePointcutAdvisor这种类型
    >
    > 判断每一个增强器是不是AspectJPointcutAdvisor这种类型，如果是，那么返回true

    

postProcessAfterInitialization()：

+ wrapIfNecessary()

  + 调用getAdvicesAndAdvisorsForBean()方法获取当前bean的所有增强器

  + 找到候选的所有增强器，也就是说是来找哪些通知方法是需要切入到当前bean的目标方法中的，添加到名为eligibleAdvisors的LinkedList集合里面

    > 用PointcutAdvisor（切入点表达式）开始来算一下每一个通知方法能不能匹配上

  + 对增强器做了一些排序

+ 将当前bean添加到名为advisedBeans的Map集合中，表示这个当前bean已经被增强处理了
+ 若当前bean需要增强，则创建当前bean的代理对象，createProxy()
  + 获取AOP代理的创建工厂
  + 工厂调用createAopProxy()方法来创建AOP代理
    + Spring会自动决定，是为组件创建jdk的动态代理呢，还是为组件创建cglib的动态代理

###### (5)、目标方法的拦截

+ 程序运行到目标方法处

+ intercept()方法

  + 拿到我们要切的目标对象

  + 获取将要执行的目标方法的拦截器链getInterceptorsAndDynamicInterceptionAdvice()

    > 所谓的拦截器链其实就是每一个通知方法又被包装为了方法拦截器

    + 先创建一个List集合，来保存所有拦截器

    + 遍历所有的增强器，将其转为Interceptor

      + 先拿到增强器，然后判断这个增强器是不是MethodInterceptor这种类型的，若是则直接添加进名为interceptors的List集合里面，若不是则使用AdvisorAdapter（增强器的适配器）将这个增强器转为MethodInterceptor这种类型，然后再添加进List集合里面，反正不管如何，最后都会将该List集合转换成MethodInterceptor数组返回出去

        > 有些通知方法是实现了MethodInterceptor接口的，也有些不是

  + 创建一个CglibMethodInvocation对象，执行其proceed()方法

    + 如果没有拦截器链，那么直接执行目标方法

      > 如果没有拦截器链，或者当前拦截器的索引和拦截器总数-1的大小一样，那么便直接执行目标方法

      + 判断currentInterceptorIndex成员变量的值（也即索引的值）是否等于this.interceptorsAndDynamicMethodMatchers.size() - 1
      + 若为空，执行目标方法，即进入invokeJoinpointUsingReflection()方法里面，利用反射来执行目标方法

    + 获取到第0号拦截器，调用当前拦截器的invoke()方法

      > 执行拦截器的invoke()方法其实就是执行CglibMethodInvocation对象的proceed()方法

      + 若该拦截器有前置任务，在此时执行

        > 前置通知调用完之后，接着是来调用目标方法的，并且目标方法调用完之后会返回到上一个拦截器（即AspectJAfterAdvice）中
        >
        > 
        >
        > 返回通知只有在目标方法运行没抛异常的时候才会被调用
        >
        > 
        >
        > 如果目标方法运行时抛异常，那么调用异常通知

      + 判断currentInterceptorIndex成员变量的值（也即索引的值）是否等于this.interceptorsAndDynamicMethodMatchers.size() - 1

      + 若相等，执行目标方法，否则获取下一个拦截器

        > 每执行一次proceed()方法，当前拦截器的索引（即currentInterceptorIndex成员变量）都会自增一次，并且还会拿到下一个拦截器。这个流程会不断地循环往复，直至拿到最后一个拦截器为止。
        >
        > 
        >
        > invoke()方法里面会调用proceed()方法，而这个proceed()方法又是寻找下一个拦截器



+ 小结：

  链式获取每一个拦截器，然后执行拦截器的invoke()方法，每一个拦截器等待下一个拦截器执行完成并返回以后，再来执行其invoke()方法。通过拦截器链这种机制，保证了通知方法与目标方法的执行顺序。

### ④、AOP原理总结

##### [1]、@EnableAspectJAutoProxy

利用@EnableAspectJAutoProxy注解来开启AOP功能

>  这个AOP功能是怎么开启的呢？
>
> 主要是通过@EnableAspectJAutoProxy注解向IOC容器中注册一个AnnotationAwareAspectJAutoProxyCreator组件来做到这点的

##### [2]、AnnotationAwareAspectJAutoProxyCreator

AnnotationAwareAspectJAutoProxyCreator组件是一个后置处理器

`IOC容器创建`

+ 创建后置处理器

首先，在创建IOC容器的过程中，会调用refresh()方法来刷新容器，而在刷新容器的过程中有一步是来注册后置处理器的，如下所示：

```java
registerBeanPostProcessors(beanFactory); // 注册后置处理器，在这一步会创建AnnotationAwareAspectJAutoProxyCreator对象
```

其实，这一步会为所有后置处理器都创建对象。



+ BeanFactory的初始化

在刷新容器的过程中还有一步是来完成BeanFactory的初始化工作的，如下所示：

```java
finishBeanFactoryInitialization(beanFactory); // 完成BeanFactory的初始化工作。所谓的完成BeanFactory的初始化工作，其实就是来创建剩下的单实例bean的。
```

很显然，剩下的单实例bean自然就包括MathCalculator（业务逻辑类）和LogAspects（切面类）这两个bean，因此这两个bean就是在这儿被创建的。

+ 创建业务逻辑组件和切面组件

+ 在这两个组件创建的过程中，最核心的一点就是AnnotationAwareAspectJAutoProxyCreator（后置处理器）会来拦截这俩组件的创建过程

+ 怎么拦截呢？主要就是在组件创建完成之后，判断组件是否需要增强。

  如需要，则会把切面里面的通知方法包装成增强器，然后再为业务逻辑组件创建一个代理对象。

  我们也认真仔细探究过了，在为业务逻辑组件创建代理对象的时候，使用的是cglib来创建动态代理的。

  当然了，如果业务逻辑类有实现接口，那么就使用jdk来创建动态代理。

  一旦这个代理对象创建出来了，那么它里面就会有所有的增强器。

+ 这个代理对象创建完以后，IOC容器也就创建完了。接下来，便要来执行目标方法了。





`执行目标方法`

此时，其实是代理对象来执行目标方法

使用CglibAopProxy类的intercept()方法来拦截目标方法的执行，拦截的过程如下：

+ 得到目标方法的拦截器链，所谓的拦截器链其实就是每一个通知方法又被包装为了方法拦截器，即MethodInterceptor

+ 利用拦截器的链式机制，依次进入每一个拦截器中进行执行

+ 最终，整个的执行效果就会有两套：
  + 目标方法正常执行：前置通知→目标方法→后置通知→返回通知
  + 目标方法出现异常：前置通知→目标方法→后置通知→异常通知



## 5、声明式事务

### ①、概述

方法上有事务，那么只要这个方法里面有任何一句代码出现了问题，该行代码之前执行的所有操作就都应该回滚。

### ②、用法

#### Ⅰ、在方法顶部标注@Transactional

#### Ⅱ、配置类上标注一个@EnableTransactionManagement注解

#### Ⅲ、将事务管理器加入IOC

```java
// 注册事务管理器在容器中
@Bean
public PlatformTransactionManager platformTransactionManager() throws Exception {
	return new DataSourceTransactionManager(dataSource());
}

```



### ③、原理

#### Ⅰ、@EnableTransactionManagement工作原理

+ @EnableTransactionManagement注解使用@Import注解给容器中引入了TransactionManagementConfigurationSelector组件

+ TransactionManagementConfigurationSelector组件向容器中快速导入两个组件，一个叫AutoProxyRegistrar，一个叫ProxyTransactionManagementConfiguration

#### Ⅱ、AutoProxyRegistrar工作原理

+ 实现接口mportBeanDefinitionRegistrar，向容器中注册bean

+ AutoProxyRegistrar向容器中注入了一个自动代理创建器，即`InfrastructureAdvisorAutoProxyCreator`

+ InfrastructureAdvisorAutoProxyCreator利用后置处理器机制在对象创建以后进行包装，然后返回一个代理对象，并且该代理对象里面会存有所有的增强器。最后，代理对象执行目标方法，在此过程中会利用拦截器的链式机制，依次进入每一个拦截器中进行执行。
+ 和之前研究AOP原理时向容器中注入的AnnotationAwareAspectJAutoProxyCreator组件所做的事情基本上没差别

#### Ⅲ、ProxyTransactionManagementConfiguration

+ 是一个配置类，它会利用@Bean注解向容器中注册各种组件

+ 配置类会利用@Bean注解向容器中注册一个事务增强器`BeanFactoryTransactionAttributeSourceAdvisor`

  + 在向容器中注册事务增强器时，它会需要一个`TransactionAttributeSource`，翻译过来应该是事务属性源
  + `TransactionAttributeSource`包含事务注解的解析器集合TransactionAnnotationParser

  > TransactionAnnotationParser有三种，分别解析：
  >
  > Spring事务注解，即org.springframework.transaction.annotation.Transactional（纯正血统，官方推荐）
  > JTA事务注解，即javax.transaction.Transactional
  > EJB 3事务注解，即javax.ejb.TransactionAttribute
  >
  > 
  >
  > TransactionAnnotationParser解析@Transactional注解里面的每一个信息

  

  + 在向容器中注册事务增强器时，除了需要事务注解信息，还需要一个事务的拦截器`TransactionInterceptor`
    + 创建完毕之后，不但会将事务属性源设置进去，而且还会将事务管理器（txManager）设置进去。也就是说，事务拦截器里面不仅保存了事务属性信息，还保存了事务管理器
    + 在代理对象执行目标方法的时候，它便会来执行拦截器链，而现在这个拦截器链，只有一个TransactionInterceptor
    + 拦截器执行invokeWithinTransaction方法
      + 先来获取事务相关的一些属性信息
      + 再来获取PlatformTransactionManager
        + 如果事务属性里面有Qualifier这个注解，并且这个注解还有值，那么就会直接从容器中按照这个指定的值来获取PlatformTransactionManager
        + 从容器中按照类型来获取一个PlatformTransactionManager
      + 如果目标方法是一个事务，那么便开启事务
      + 执行目标方法
      + 正常运行，最后执行commitTransactionAfterReturning方法，先获取到事务管理器，然后再利用事务管理器提交事务
      + 执行目标方法时出现异常，执行completeTransactionAfterThrowing方法，发先获取到事务管理器，然后再利用事务管理器回滚这次操作





# 二、扩展原理

## 1、BeanFactoryPostProcessor

BeanFactoryPostProcessor其实就是BeanFactory（创建bean的工厂）的后置处理器

在IOC容器里面的BeanFactory的标准初始化完成之后，修改IOC容器里面的这个BeanFactory



#### Ⅰ、调用时机

BeanFactoryPostProcessor的调用时机是在BeanFactory标准初始化之后，这样一来，我们就可以来定制和修改BeanFactory里面的一些内容了，此时，所有的bean定义已经保存加载到BeanFactory中了，但是bean的实例还未创建



#### Ⅱ、执行流程

+ 在刷新容器的过程中，还得执行在容器中注册的BeanFactoryPostProcessor（BeanFactory的后置处理器）的方法
+ PostProcessorRegistrationDelegate类中的invokeBeanFactoryPostProcessors方法，遍历了所有的BeanFactoryPostProcessor组件



## 2、BeanDefinitionRegistryPostProcessor

BeanFactoryPostProcessor旗下的一个子接口

#### Ⅰ、执行时机

postProcessBeanDefinitionRegistry方法的执行时机是在所有bean定义信息将要被加载，但是bean实例还未创建的时候。

BeanFactoryPostProcessor的执行时机是在所有的bean定义信息已经保存加载到BeanFactory中之后，而BeanDefinitionRegistryPostProcessor却是在所有的bean定义信息将要被加载的时候



#### Ⅱ、执行流程

+ 在IOC容器创建对象的时候，会来调用invokeBeanFactoryPostProcessors这个方法

+ 从IOC容器中获取到所有的BeanDefinitionRegistryPostProcessor组件，并依次触发它们的postProcessBeanDefinitionRegistry方法，然后再来触发它们的postProcessBeanFactory方法

+ 再来从IOC容器中获取到所有的BeanFactoryPostProcessor组件，并依次触发它们的postProcessBeanFactory方法



## 3、ApplicationListener

#### Ⅰ、作用

它的作用主要是来监听IOC容器中发布的一些事件，只要事件发生便会来触发该监听器的回调，从而来完成事件驱动模型的开发。

#### Ⅱ、用法

```java
@Component
public class MyApplicationListener implements ApplicationListener<ApplicationEvent> {

	// 当容器中发布此事件以后，下面这个方法就会被触发
	@Override
	public void onApplicationEvent(ApplicationEvent event) {
		// TODO Auto-generated method stub
		System.out.println("收到事件：" + event);
	}

}
```

#### Ⅲ、发布事件

IOC容器在刷新完成之后便会发布ContextRefreshedEvent事件，一旦容器关闭了便会发布ContextClosedEvent事件

可以自己发布事件

#### Ⅳ、事件发布流程

+ 刷新容器，之后调用finishRefresh方法
+ 调用的finishRefresh方法里面又调用了一个叫publishEvent的方法，而且传递进该方法的参数是new出来的一个ContextRefreshedEvent对象
+ finishRefresh方法调用一个getApplicationEventMulticaster方法，获取事件多播器
+ 调用事件多播器的multicastEvent方法，向各个监听器派发事件
+ 一个for循环，在这个for循环中，有一个getApplicationListeners方法，它是来拿到所有的ApplicationListener的，拿到之后就会来挨个遍历再来拿到每一个ApplicationListener
+ 判断getTaskExecutor方法能不能够返回一个Executor对象，如果能够，那么会利用Executor的异步执行功能来使用多线程的方式异步地派发事件；如果不能够，那么就使用同步的方式直接执行ApplicationListener的方法
+ 调用了一个叫invokeListener的方法，而且在该方法中传入了当前遍历出来的ApplicationListener
+ 回调它的onApplicationEvent方法
+ 来到监听器（例如MyApplicationListener）中
+ 回调它其中的onApplicationEvent方法



#### Ⅴ、从哪获取到的事件多播器

+ 在创建容器的过程中，还会调用一个refresh方法来刷新容器
+ 在初始化创建其他组件之前调用initApplicationEventMulticaster的方法
+ 判断IOC容器（也就是BeanFactory）中是否有id等于applicationEventMulticaster的组件
+ 如果IOC容器中有id等于applicationEventMulticaster的组件，那么就会通过getBean方法直接拿到这个组件；如果没有，那么就重新new一个SimpleApplicationEventMulticaster类型的事件多播器，然后再把这个事件多播器注册到容器中



#### Ⅵ、监听器注册到事件多播器

+ 刷新容器
+ 在initApplicationEventMulticaster之后调用registerListeners的方法
+ 从容器中拿到所有的监听器，然后再把它们注册到applicationEventMulticaster



#### Ⅶ、@EventListener

##### [1]、作用

@EventListener注解，我们就可以让任意方法都能监听事件

##### [2]、用法

```java
@EventListener(classes=ApplicationEvent.class)
	public void listen() {
		System.out.println("UserService...");
	}
```

```java
@EventListener(classes={ApplicationEvent.class})
	public void listen(ApplicationEvent event) {
		System.out.println("UserService...监听到的事件：" + event);
	}
```

#### [3]、原理

+ Spring会使用`EventListenerMethodProcessor`这个处理器来解析方法上的@EventListener注解
+ `EventListenerMethodProcessor`的afterSingletonsInstantiated方法，在所有的单实例bean已经全部被创建完了以后才会被执行
+ 调用时机：刷新容器->finishBeanFactoryInitialization方法完成BeanFactory的初始化工作->调用preInstantiateSingletons方法：创建所有的单实例bean->获取所有创建好的单实例bean，然后判断每一个bean对象是否是SmartInitializingSingleton这个接口类型的，如果是，那么便调用它里面的afterSingletonsInstantiated方法->调用finishRefresh方法





## 4、Spring容器创建过程

Spring容器的创建以及初始化过程，关注点放在refresh方法上，也即刷新容器。该方法运行完以后，容器就创建完成了，包括所有的bean对象也都创建和初始化完成了

该方法的工作主要有这些部分：

+ BeanFactory的创建以及预准备工作①~④
+ 利用BeanFactory初始化一些特殊组件⑤~⑩
+ 初始化剩下的单例bean①①~①②



------



### ①、prepareRefresh()

#### Ⅰ、概述

刷新容器前的预处理工作

#### Ⅱ、内容

先记录下当前时间，然后设置下当前容器是否是关闭、是否是活跃的等状态，除此之外，还会打印当前容器的刷新日志

##### [1]、initPropertySources()

子类自定义个性化的属性设置的方法

该方法是protected类型的，这意味着它是留给子类自定义个性化的属性设置的

##### [2]、getEnvironment().validateRequiredProperties()

获取其环境变量，然后校验属性的合法性

> 使用属性解析器来进行属性校验的

##### [3]、保存容器中早期的事件

new了一个LinkedHashSet，它主要是来临时保存一些容器中早期的事件的。如果有事件发生，那么就存放在这个LinkedHashSet里面，这样，当事件派发器好了以后，直接用事件派发器把这些事件都派发出去。



### ②、obtainFreshBeanFactory()

#### Ⅰ、概述

获取BeanFactory对象

##### Ⅱ、内容

##### (1)、refreshBeanFactory()

创建BeanFactory对象，并为其设置一个序列化id

> 其实在创建GenericApplicationContext对象时，无参构造器里面就new出来了beanFactory这个对象

##### (2)、getBeanFactory()

返回设置了序列化id后的BeanFactory对象



### ③、prepareBeanFactory(beanFactory)

#### Ⅰ、概述

BeanFactory的预准备工作，即对BeanFactory进性一些预处理

#### Ⅱ、内容

##### (1)、设置属性

对BeanFactory进行一系列的赋值（即设置一些属性）

##### (2)、添加部分后置处理器

向BeanFactory中添加了一个BeanPostProcessor，即ApplicationContextAwareProcessor

##### (3)、设置忽略的接口

为BeanFactory设置忽略的自动装配的接口

这些接口的实现类不能通过接口类型来自动注入

##### (4)、注册自动装配

为BeanFactory注册可以解析的自动装配

> 所谓的可以解析的自动装配，就是说，我们可以直接在任何组件里面自动注入像BeanFactory、ResourceLoader、ApplicationEventPublisher（它就是上一讲我们讲述的事件派发器）以及ApplicationContext（也就是我们的IOC容器）这些东西

##### (5)、继续添加后置处理器

添加的是一个叫ApplicationListenerDetector的BeanPostProcessor。

##### (6)、添加功能

判断语句，它这是向BeanFactory中添加编译时与AspectJ支持相关的东西

##### (7)、注册系统bean

向BeanFactory中注册一些与环境变量相关的bean，比如注册了一个名字是environment，值是当前环境对象（其类型是ConfigurableEnvironment）的bean。



### ④、postProcessBeanFactory(beanFactory)

#### Ⅰ、概述

BeanFactory准备工作完成后进行的后置处理工作

方法里面都是空的，默认都是不进行任何处理的，但是方法都是protected类型的，这也就是说子类可以通过重写这个方法，在BeanFactory创建并预处理完成以后做进一步的设置。

至此，BeanFactory的创建以及预准备工作就已经完成



------



### ⑤、invokeBeanFactoryPostProcessors

#### Ⅰ、概述

+ 执行BeanFactoryPostProcessor

+ BeanFactoryPostProcessor（也可以说它里面的那个方法）是在BeanFactory标准初始化之后执行的

+ invokeBeanFactoryPostProcessors方法最主要的核心作用就是执行了BeanDefinitionRegistryPostProcessor的postProcessBeanDefinitionRegistry和postProcessBeanFactory这俩方法，然后执行BeanFactoryPostProcessors的postProcessBeanFactory方法

+ 即BeanDefinitionRegistryPostProcessor是要优先于BeanFactoryPostProcessor执行的

#### Ⅱ、内容

##### [1]、getBeanFactoryPostProcessors

返回了一个空的List<BeanFactoryPostProcessor>集合，该集合是用于存放所有的BeanFactoryPostProcessor

##### [2]、invokeBeanFactoryPostProcessors(beanFactory, BeanFactoryPostProcessors)

+ 判断beanFactory是不是BeanDefinitionRegistry

+ 循环遍历invokeBeanFactoryPostProcessors方法中的第二个参数的，即beanFactoryPostProcessors
  + 以遍历出来的每一个BeanFactoryPostProcessor是否实现了BeanDefinitionRegistryPostProcessor接口为依据将其分别存放于以下两个箭头所指向的LinkedList中，其中实现了BeanDefinitionRegistryPostProcessor接口的还会被直接调用

+ 拿到所有BeanDefinitionRegistryPostProcessor的这些bean的名字
  + 第一次获取容器中所有实现了BeanDefinitionRegistryPostProcessor接口的组件时，其实只能获取到ConfigurationClassPostProcessor，因为我们手工加的只是BeanDefinition，等ConfigurationClassPostProcessor把对应的Definition加载后，下面才能获取到我们手工加载的BeanDefinition。
+ 判断每一个BeanDefinitionRegistryPostProcessor组件是不是实现了PriorityOrdered这个优先级接口，若是，则会先按照优先级排个序，然后再调用该组件的postProcessBeanDefinitionRegistry方法。
  + 执行BeanDefinitionRegistryPostProcessor组件的postProcessBeanDefinitionRegistry方法
+ 执行BeanDefinitionRegistryPostProcessor的postProcessBeanFactory方法，至此，BeanDefinitionRegistryPostProcessor执行完毕
+ 接下来获取到所有BeanFactoryPostProcessor组件
+ 遍历所有这些BeanFactoryPostProcessor组件了，挨个遍历出来之后，按照是否实现了PriorityOrdered接口、Ordered接口以及没有实现这两个接口这三种情况进行分类，将其分别存储于三个ArrayList中
+ 按照顺序依次执行BeanFactoryPostProcessors组件对应的postProcessBeanFactory方法



### ⑥、registerBeanPostProcessors

#### Ⅰ、概述

注册BeanPostProcessor的，即注册bean的后置处理器

向beanFactory中添加了所有这些bean的后置处理器，而并不会执行它们

#### Ⅱ、内容

##### [1]、获取BeanPostProcessor

BeanPostProcessor接口旗下有非常多的子接口，而且这些不同接口类型的BeanPostProcessor在bean创建前后的执行时机是不一样的，虽然它们都是后置处理器。

> DestructionAwareBeanPostProcessor：它是销毁bean的后置处理器
> InstantiationAwareBeanPostProcessor
> SmartInstantiationAwareBeanPostProcessor
> MergedBeanDefinitionPostProcessor

##### [2]、添加BeanPostProcessorChecker

添加了一个BeanPostProcessorChecker类型的后置处理器，它是来检查所有BeanPostProcessor组件

##### [3]、按次序注册BeanPostProcessor

将所有的BeanPostProcessor组件分门别类之后，依次存储在不同的ArrayList集合中

##### [4]、添加ApplicationListenerDetector

在bean创建初始化之后，探测该bean是不是ApplicationListener



### ⑦、initMessageSource

#### Ⅰ、概述

初始化MessageSource组件的。对于Spring MVC而言，该方法主要是来做国际化功能的，如消息绑定、消息解析等。

#### Ⅱ、内容

##### [1]、获取BeanFactory

##### [2]、判断

看容器中是否有id为messageSource，类型是MessageSource的组件

若有，则赋值给this.messageSource

若没有，则创建一个DelegatingMessageSource类型的组件，并把创建好的组件注册在容器中

> MessageSource类型的组件的作用一般是取出国际化配置文件中某个key所对应的值



### ⑧、initApplicationEventMulticaster

#### Ⅰ、概述

初始化事件派发器，对我们Spring中的事件进行一些派发、管理以及通知

#### Ⅱ、内容

##### [1]、获取BeanFactory

##### [2]、判断

看容器中是否有id为applicationEventMulticaster，类型是ApplicationEventMulticaster的组件

若有，则赋值给this.applicationEventMulticaster

若没有，则创建一个SimpleApplicationEventMulticaster类型的组件，并把创建好的组件注册在容器中



### ⑨、onRefresh()

#### Ⅰ、概述

空方法，当子类（也可以说是子容器）重写该方法后，在容器刷新的时候就可以再自定义一些逻辑了，比如给容器中多注册一些组件之类的



### ⑩、registerListeners()

#### Ⅰ、概述

将我们项目里面的监听器（也即咱们自己编写的ApplicationListener）注册进来

#### Ⅱ、内容

##### [1]、循环

for循环，该for循环是来遍历从容器中获取到的所有的ApplicationListener的，然后将遍历出的每一个监听器添加到事件派发器中

##### [2]、getBeanNamesForType

从容拿到所有的ApplicationListener组件，添加到事件派发器中



------



### ①①、finishBeanFactoryInitialization(beanFactory)

#### Ⅰ、概述

初始化所有剩下的单实例bean，即调用preInstantiateSingletons()完成

#### Ⅱ、内容

##### [1]、循环

该for循环是来遍历容器中所有的bean，然后依次触发它们的整个初始化逻辑

###### (1)、获取bean的定义注册信息

bean的定义注册信息是需要用RootBeanDefinition这种类型来进行封装

###### (2)、判断

判断bean是否是抽象的、单实例的、懒加载的

###### (3)、判断通过

+ 判断当前bean是不是FactoryBean，如果实现了FactoryBean接口，那么Spring就会调用FactoryBean接口里面的getObject方法来帮我们创建对象

+ 如果我们的bean并没有实现FactoryBean接口，那么就会利用getBean方法来创建对象

  + 调用了一个叫doGetBean的方法

    + 拿到我们的bean的名字

    + 根据我们bean的名字尝试获取缓存中保存的单实例bean

      > 创建过的单实例bean都会被缓存起来，所以这儿会调用getSingleton方法先从缓存中获取。如果能获取到，那么说明这个单实例bean之前已经被创建过了。

    + 如果从缓存中获取不到我们的bean，调用创建

      + 获取一个（父）BeanFactory

      + markBeanAsCreated方法，标记其为已创建，相当于做了一个小标记，这主要是为了防止多个线程同时来创建同一个bean，从而保证了bean的单实例特性

      + getDependsOn，获取我们当前bean所依赖的其他bean

      + 调用createBean方法启动创建

        + 拿到我们bean的定义信息，然后再来解析我们要创建的bean的类型。

        + 调用了一个resolveBeforeInstantiation方法

        + 给BeanPostProcessor一个机会来提前返回我们bean的代理对象，这主要是为了解决依赖注入问题。也就是说，这是让BeanPostProcessor先拦截并返回代理对象。

        + 判断是否有InstantiationAwareBeanPostProcessor这种类型的后置处理器的。如果有，那么就会来执行InstantiationAwareBeanPostProcessor这种类型的后置处理器

          + applyBeanPostProcessorsBeforeInstantiation方法
          + 如果applyBeanPostProcessorsBeforeInstantiation方法执行完之后返回了一个对象，并且还不为null，那么紧接着就会来执行后面的applyBeanPostProcessorsAfterInitialization方法。
          + 完成创建

        + 如果未返回代理对象，调用doCreateBean创建

          + BeanWrapper接口来接收我们创建的bean

          + 创建bean实例

            + getFactoryMethodName方法，该方法是来获取工厂方法
            + 工厂利用反射来创建对象

          + 后置处理器修改bean的定义信息

            + 遍历获取到的所有后置处理器，若是MergedBeanDefinitionPostProcessor这种类型，则调用其postProcessMergedBeanDefinition方法

              > 每一个后置处理器（或者说它里面的方法）的执行时机都是不一样的，比如我们在上一讲中所讲述的InstantiationAwareBeanPostProcessor这种类型的后置处理器中的两个方法的执行时机是在创建bean实例之前，而现在MergedBeanDefinitionPostProcessor这种类型的后置处理器，是在创建完bean实例以后，来执行它里面的postProcessMergedBeanDefinition方法的

          + 调用populateBean方法，是来为bean的属性赋值的

            + 遍历获取到的所有后置处理器，若是InstantiationAwareBeanPostProcessor这种类型，则调用其postProcessAfterInstantiation方法，postProcessPropertyValues方法

              > postProcessPropertyValues方法返回一个PropertyValues对象，它就是我们bean实例属性要被赋予的属性值

            + 调用applyPropertyValues方法，利用setter方法为bean的属性进行赋值

          + 调用initializeBean，初始化bean

            + 调用了invokeAwareMethods方法，执行XxxAware接口中的方法，判断我们的bean是不是实现了BeanNameAware、BeanClassLoaderAware、BeanFactoryAware这些Aware接口的，若是则回调接口中对应的方法
            + 调用postProcessBeforeInitialization方法，遍历所有的后置处理器，然后依次执行所有后置处理器的postProcessBeforeInitialization方法，一旦后置处理器的postProcessBeforeInitialization方法返回了null以后，则后面的后置处理器便不再执行了，而是直接退出for循环。
            + invokeInitMethods方法，其作用就是执行初始化方法。
            + 调用applyBeanPostProcessorsAfterInitialization方法，遍历所有的后置处理器，然后依次执行所有后置处理器的postProcessAfterInitialization方法，一旦后置处理器的postProcessAfterInitialization方法返回了null以后，则后面的后置处理器便不再执行了，而是直接退出for循环。

          + 调用registerDisposableBeanIfNecessary方法，注册bean的销毁方法

        + 调用一个addSingleton方法，将创建的单实例bean添加到缓存中，singletonObjects属性就是一个Map集合，该Map集合里面缓存的就是所有的单实例bean

+ getBean方法到此结束，单实例bean创建完成

+ 至此，所有bean创建完成

+ 检查所有的bean中是否有实现SmartInitializingSingleton接口的，如果有的话，那么便会来执行该接口中的afterSingletonsInstantiated方法。



### ①②、finishRefresh

#### Ⅰ、概述

finishRefresh方法执行完，就意味着完成了BeanFactory的初始化创建工作，顺带脚地，我们Spring IOC容器就创建完成了

#### Ⅱ、内容

##### [1]、initLifecycleProcessor

初始化和生命周期有关的后置处理器

+ 获取BeanFactory

+ 看容器中是否有id为lifecycleProcessor，类型是LifecycleProcessor的组件

  > 该接口中还定义了两个方法，一个是onRefresh方法，一个是onClose方法
  >
  > 可以在BeanFactory的生命周期期间进行拦截，即在BeanFactory刷新完成以及关闭的时候，回调其里面的onRefresh和onClose这俩方法

+ 若有，则赋值给this.lifecycleProcessor

+ 若没有，则创建一个DefaultLifecycleProcessor类型的组件，并把创建好的组件注册在容器中





# 三、web

## 1、servlet3.0

### ①、@WebServlet

等同于以前将编写好的Servlet配置在web.xml文中，例如配置一下其拦截路径等等

可以在该注解中配置要拦截哪些路径，例如@WebServlet("/hello")，这样就会拦截一个hello请求



### ②、 runtimes pluggability

#### Ⅰ、概述

即运行时插件能力

container（即Servlet容器，比如Tomcat服务器之类的）在启动我们的应用的时候，它会来扫描jar包里面的ServletContainerInitializer的实现类。

Servlet容器在启动应用的时候，会扫描当前应用每一个jar包里面的META-INF/services/javax.servlet.ServletContainerInitializer文件中指定的实现类，然后，再运行该实现类中的方法。

#### Ⅱ、用法

##### [1]、创建实现类

创建ServletContainerInitializer的实现类，重写onStartup方法

该方法里面有两个参数，其中一个参数是ServletContext对象

第二个参数，即Set<Class<?>> arg0

> 在ServletContainerInitializer的实现类上使用一个@HandlesTypes注解，而且在该注解里面我们可以写上一个类型数组，Servlet容器在启动应用的时候，会将@HandlesTypes注解里面指定的类型下面的子类，包括实现类或者子接口等，全部给我们传递过来

##### [2]、全类名配置

全类名配置在META-INF/services目录下的javax.servlet.ServletContainerInitializer文件中



### ③、注册web三大组件

#### Ⅰ、注册Servlet

```java
	@Override
	public void onStartup(Set<Class<?>> arg0, ServletContext sc) throws ServletException {
		// 注册Servlet组件
		ServletRegistration.Dynamic servlet = sc.addServlet("userServlet", new 
                                                            UserServlet());
        // 配置Servlet的映射信息
		servlet.addMapping("/user");
	}
```



#### Ⅱ、注册Listener

```java
		// 注册Listener组件
		sc.addListener(UserListener.class);
```



#### Ⅲ、注册Filter

```java
		// 注册Filter组件
		FilterRegistration.Dynamic filter = sc.addFilter("userFilter", 
                                                         UserFilter.class);
		// 配置Filter的映射信息
		filter.addMappingForUrlPatterns(EnumSet.of(DispatcherType.REQUEST), true, 
                                        "/*");
```

> EnumSet.of(DispatcherType.REQUEST)，该参数表示的是Filter拦截的请求类型，即通过什么方式过来的请求，Filter会进行拦截



### ④、MVC整合

#### Ⅰ、无web.xml

即使没有web.xml文件，也不要报错误

```xml
	<packaging>war</packaging>	
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-war-plugin</artifactId>
				<version>2.4</version>
				<configuration>
					<failOnMissingWebXml>false</failOnMissingWebXml>
				</configuration>
			</plugin>
		</plugins>
	</build>
```

#### Ⅱ、基于父子容器

```java
// 我们可以编写一个类来实现WebApplicationInitializer接口哟，当然了，你也可以编写一个类来实现ServletContainerInitializer接口
public class MyWebApplicationInitializer implements WebApplicationInitializer {

	@Override
	public void onStartup(ServletContext servletContext) {

		// 然后，我们来创建一个AnnotationConfigWebApplicationContext对象，它应该代表的是web的IOC容器
		AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
		// 加载我们的配置类
		context.register(AppConfig.class);

		// 在容器启动的时候，我们自己来创建一个DispatcherServlet对象，并将其注册在ServletContext中
		DispatcherServlet servlet = new DispatcherServlet(context);
		ServletRegistration.Dynamic registration = servletContext.addServlet("app", servlet);
		registration.setLoadOnStartup(1);
		// 这儿是来配置DispatcherServlet的映射信息的
		registration.addMapping("/app/*");
	}
}

```



#### Ⅲ、基于运行时插件能力

+ Servlet容器在启动应用的时候，会扫描当前应用每一个jar包里面的META-INF/services/javax.servlet.ServletContainerInitializer文件中指定的实现类，然后加载该实现类并运行它里面的方法。

+ 指定的是org.springframework.web.SpringServletContainerInitializer这个类
+ 里面也只有一个onStartup方法
+ Servlet容器在启动我们Spring应用之后，会传入一个我们感兴趣的类型的集合，然后在onStartup方法中拿到之后就会来挨个遍历，如果遍历出来的我们感兴趣的类型不是接口，也不是抽象类，但是WebApplicationInitializer接口旗下的，那么就会创建该类型的一个实例，并将其存储到名为initializers的LinkedList<WebApplicationInitializer>集合中

##### [1]、三大抽象类

###### (1)、AbstractContextLoaderInitializer

+ 注册ContextLoaderListener
+ 调用了一个createRootApplicationContext方法，该方法是来创建根容器的，该方法是一个抽象方法，需要子类自己去实现。
+ 然后，根据创建的根容器创建上下文加载监听器（即ContextLoaderListener），接着，向ServletContext中注册这个监听器

###### (2)、AbstractDispatcherServletInitializer

+ AbstractContextLoaderInitializer的子类，注册DispatcherServlet。
+ 先调用createServletApplicationContext方法来创建一个WebApplicationContext（即web的IOC容器），再调用createDispatcherServlet方法来创建一个DispatcherServlet，将创建好的DispatcherServlet注册到ServletAppContext中。

+ 配置该DispatcherServlet的映射信息时，还调用了一个getServletMapppings方法，不过该方法是一个抽象方法，需要由子类自己来重写。



(3)、AbstractAnnotationConfigDispatcherServletInitializer

+ 重写了AbstractContextLoaderInitializer抽象父类里面的createRootApplicationContext方法，创建根容器
  + 首先获取到一个配置类，调用的可是getRootConfigClasses方法，调用该方法能传入一个配置类（其实就是我们自己写的），而且该方法还是一个抽象方法，需要由子类自己来重写。
  + 继续，要知道我们以前写的可是xml配置文件，获取到了之后，会new一个AnnotationConfigWebApplicationContext，这就相当于创建了一个根容器，然后将获取到的配置类注册进去，相当于是注册配置类里面的组件，最终返回创建的根容器。
+ 该抽象类里面还有一个createServletApplicationContext方法，如下所示，它是来创建web的IOC容器的，其实这个方法就是重写的AbstractDispatcherServletInitializer抽象类里面的createServletApplicationContext方法。
  + 首先会创建一个web的IOC容器（即AnnotationConfigWebApplicationContext对象），然后再获取一个配置类，调用的是getServletConfigClasses方法
  + 获取到配置类之后，最终会将其注册进去。

##### [2]、整合步骤

###### (1)、继承AbstractAnnotationConfigDispatcherServletInitializer

```java
public class MyWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

	@Override
	protected Class<?>[] getRootConfigClasses() {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	protected Class<?>[] getServletConfigClasses() {
		// TODO Auto-generated method stub
		return null;
	}

	@Override
	protected String[] getServletMappings() {
		// TODO Auto-generated method stub
		return null;
	}

}
```

###### (2)、重写getRootConfigClasses方法

它是来获取根容器的配置类的，该配置类就类似于我们以前经常写的Spring的配置文件

```java
return new Class<?>[]{RootConfig.class};
```



###### (3)、重写getServletConfigClasses方法

它是来获取web容器的配置类的，该配置类就类似于我们以前经常写的Spring MVC的配置文件

```java
return new Class<?>[]{AppConfig.class};
```



###### (4)、重写getServletMappings方法

它是来获取DispatcherServlet的映射信息的。该方法需要返回一个String[]

```java
return new String[]{"/"};
```



#### Ⅳ、定制MVC

##### [1]、@EnableWebMvc注解

写一个配置类，然后将@EnableWebMvc注解标注在该配置类上

> 相当于我们以前在xml配置文件中加上了<mvc:annotation-driven/>这样一个配置

##### [2]、实现WebMvcConfigurer接口

实现WebMvcConfigurer接口，就可以来定制配置

> 配置类只要实现了WebMvcConfigurer接口，那么你就得一个一个来实现其中的方法

##### [3]、配置MVC

+ 配置路径映射规则

  ```java
  @Override
  public void configurePathMatch(PathMatchConfigurer configurer) {
      // TODO Auto-generated method stub
      
  }
  ```

+ 开启异步支持

  ```java
  @Override
  public void configureAsyncSupport(AsyncSupportConfigurer configurer) {
      // TODO Auto-generated method stub
      
  }
  ```

+ 开启静态资源

  ```java
  @Override
  public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
      // TODO Auto-generated method stub
      configurer.enable()
  }
  ```

  > 相当于我们以前在xml配置文件中写上<mvc:default-servlet-handler/>

+ 添加拦截器

  ```java
  @Override
  public void addInterceptors(InterceptorRegistry registry) {
      // TODO Auto-generated method stub
      
  }
  ```

  

##### [4]、继承WebMvcConfigurerAdapter抽象类

该抽象类把WebMvcConfigurer接口中的方法都实现了，只不过每一个方法里面都是空的而已

##### [5]、定制Spring MVC

+ 视图解析器

  ```java
  // 定制视图解析器
  	@Override
  	public void configureViewResolvers(ViewResolverRegistry registry) {
  		// TODO Auto-generated method stub
  		// super.configureViewResolvers(registry); 注释掉这行代码，因为其父类中的方法都是空的
  		
  		// 如果直接调用jsp方法，那么默认所有的页面都从/WEB-INF/目录下开始找，即找所有的jsp页面
  		// registry.jsp();
  		
  		/*
  		 * 当然了，我们也可以自己来编写规则，比如指定一个前缀，即/WEB-INF/views/，再指定一个后缀，即.jsp，
  		 * 很显然，此时，所有的jsp页面都会存放在/WEB-INF/views/目录下，自然地，程序就会去/WEB-INF/views/目录下面查找jsp页面了
  		 */
  		registry.jsp("/WEB-INF/views/", ".jsp");
  	}
  ```

+ 静态资源

  ```java
  	// 定制静态资源的访问
  	@Override
  	public void configureDefaultServletHandling(DefaultServletHandlerConfigurer 
                                                  configurer) {
  		configurer.enable();
  	}
  ```

+ 定制拦截器

  ```java
  // 定制拦截器
  	@Override
  	public void addInterceptors(InterceptorRegistry registry) {
  		// TODO Auto-generated method stub
  		// super.addInterceptors(registry);
  		
  		/*
  		 * addInterceptor方法里面要传一个拦截器对象，该拦截器对象可以从容器中获取过来，也可以我们自己来new一个，
  		 * 很显然，这儿我们是new了一个我们自定义的拦截器对象。
  		 * 
  		 * 虽然创建出了一个拦截器，但是最关键的一点还是指示拦截器要拦截哪些请求，因此还得继续使用addPathPatterns方法来配置一下，
  		 * 若在addPathPatterns方法中传入了"/**"，则表示拦截器会拦截任意请求，而不管该请求是不是有任意多层路径
  		 */
  		registry.addInterceptor(new 
                                  MyFirstInterceptor()).addPathPatterns("/**");
  	}
  ```

  



## 2、异步请求

### ①、默认的请求处理机制Thread-Per-Request

#### Ⅰ、处理过程

一个请求进来，Tomcat服务器里面会有一个线程池来进行处理请求，请求一进来，Tomcat服务器就从线程池中拿到一个空闲的线程来帮我们处理请求。请求处理时，必然就会调用业务逻辑，调完之后，就代表请求已经处理完了，接着就会给客户端一个响应，响应结束以后，整个线程就会得到释放，线程又会进入空闲状态，等待接收下一个请求

#### Ⅱ、图示

![image-20220716165609202](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220716165609202.png)

#### Ⅲ、缺点

Tomcat服务器中的线程池里面的线程数量是有限的，如果在没有异步处理的情况下，假设同时进来几百个请求，并且还假设线程池中的每一个线程都在处理请求，这时，正是由于没有异步处理，而服务器端的业务逻辑一旦调用很长一段的时间，那么下一个请求再要进来，Tomcat服务器中的主线程池里面就没有再多空闲的线程来处理了。请求没法得到处理的本质原因就是主线程没有及时得到释放，即没有更多的空闲线程来进行处理请求



### ②、Servlet3.0异步处理

#### Ⅰ、开启异步处理

```java
@WebServlet(value="/async", asyncSupported=true) 
public class HelloAsyncServlet extends HttpServlet {
}
```

#### Ⅱ、开启异步模式

```java
@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// 1. 先来让该Servlet支持异步处理，即asyncSupported=true
		// 2. 开启异步模式
		AsyncContext startAsync = req.startAsync();
	}
```

#### Ⅲ、设置返回内容

可以给返回的异步的AsyncContext对象里面设置一些东西。例如设置异步的监听器

#### Ⅳ、调用业务逻辑

```java
// 3. 调用业务逻辑，进行异步处理，这儿是开始异步处理
		startAsync.start(new Runnable() {
			
			@Override
			public void run() {
				// TODO Auto-generated method stub
				try {
					sayHello();
				} catch (Exception e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		});
```

#### Ⅴ、获取响应

```java
				try {
					sayHello();
					
					startAsync.complete();
					/*
					 * 通过下面这种方式来获取响应对象是不可行的哟！否则，会报如下异常：
					 * java.lang.IllegalStateException: It is illegal to call this method if the current request is not in asynchronous mode (i.e. isAsyncStarted() returns false)
					 */
					// 获取到异步上下文
					//AsyncContext asyncContext = req.getAsyncContext();
					// ServletResponse response = asyncContext.getResponse();
					
					// 4. 获取响应
					ServletResponse response = startAsync.getResponse();
					// 然后，我们还是利用这个响应往客户端来写数据
					response.getWriter().write("hello async...");
                }
```

#### Ⅵ、处理机制图示

![image-20220716170810799](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220716170810799.png)

### ③、Spring MVC异步处理

#### Ⅰ、返回值写成Callable

##### [1]、写法

```java
@Controller
public class AsyncController {

	@ResponseBody
	@RequestMapping("/async01")
	public Callable<String> async01() {
		Callable<String> callable = new Callable<String>() {

			@Override
			public String call() throws Exception {
				// 响应给客户端一串字符串，即"Callable<String> async01()"
				return "Callable<String> async01()";
			}
			
		};
		return callable;
	}
	
}
```

##### [2]、处理流程

+ 控制器返回Callable
+ 控制器返回Callable以后，Spring MVC就会异步地启动一个处理方法（即Spring MVC异步处理），也即将Callable提交到TaskExecutor（任务执行器）里面，并使用一个隔离的线程进行处理
+ DispatcherServlet和所有的Filter将会退出Servlet容器的线程（即主线程），但是response仍然保持打开的状态
+ Callable返回一个结果，并且Spring MVC会将请求重新派发给Servlet容器，恢复之前的处理。
+ 把上一次的请求再发过来，DispatcherServlet便会再次执行，来恢复之前的处理
+ 根据Callable返回的结果，该怎么处理还是怎么处理，即Spring MVC会继续进入视图渲染等等流程，也就是说，Spring MVC会从头开始再次执行其流程（收请求→视图渲染→给响应），这是因为请求会被再次发给Spring MVC

##### [3]、注意

在异步处理的情况下，Spring MVC并不能拦截到真正的业务逻辑的整个处理流程，而想要做到这一点，那就得使用异步的拦截器

+ 原生Servlet里面的AsyncListener
+ Spring MVC给提供的异步拦截器，即AsyncHandlerInterceptor



#### Ⅱ、返回值类型要写成DeferredResult

##### [1]、第一种方式的缺陷

在实际的开发过程中，异步请求处理是绝不可能这么简单

以创建订单为例，创建订单的请求一进来，应用1就要启动一个线程，来帮我们处理这个请求。如果假设应用1并不能创建订单，创建订单需要应用2来完成，那么此时应该怎么办呢？应用1可以把创建订单的消息存放在消息中间件中，比如RabbitMQ、Kafka等等，而应用2就来负责监听这些消息中间件里面的消息，一旦它发现有创建订单这个消息，那么它就进行相应处理，然后将处理完成后的结果，比如订单的订单号等等，再次存放在消息中间件中，接着应用1再启动一个线程，例如线程2，来监听消息中间件中的返回结果，只要订单创建完毕，它就会拿到返回的结果（即订单的订单号），最后将其响应给客户端。

![image-20220716171925533](https://ningct.oss-cn-hangzhou.aliyuncs.com/image-20220716171925533.png)

##### [2]、DeferredResult原理

当客户端发送一个请求过来以后，即使该请求不能及时得到处理，那也没有关系，你可以先new一个DeferredResult对象，然后把该对象返回出去，并把该对象保存在其他的地方，此时请求依旧在等待处理的过程中，那什么时候这个请求能被处理完成呢？这得等到另外一个线程在其他的地方获取到DeferredResult对象，并调用其setResult方法设置了结果之后，此时请求才会被处理完成，并响应给客户端。

##### [3]、写法

```java
	@ResponseBody
	@RequestMapping("/createOrder")
	public DeferredResult<Object> createOrder() {
		/*
		* 在创建DeferredResult对象时，可以像下面这样传入一些参数
		* 
		* 第一个参数（timeout）： 超时时间。限定（请求？）必须在该时间内执行完，如果超出时间限制，那么就会返回一段错误的提示信息（timeoutResult）
		* 第二个参数（timeoutResult）：超出时间限制之后，返回的错误提示信息
		*/
		DeferredResult<Object> deferredResult = new DeferredResult<>((long)3000, "create fail...");
		DeferredResultQueue.save(deferredResult);
		return deferredResult;
	}
```

##### [4]、响应客户端

另外一个线程拿到临时保存的DeferredResult对象之后，只要将最终处理的结果给该对象设置进去，那么另一边的线程就能立即得到返回结果了。