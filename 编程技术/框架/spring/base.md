## IoC

Inverse of Control是一种设计思想，就是将原本程序中手动创建的对象控制权，交由Spring框架来管理，IoC容器实际上就是个Map，里面存放的是各种对象

Spring IoC初始化，以XML配置为例

```
XML —> Resource —> BeanDefinition —> BeanFactory
```

Spring IoC容器就像一个工厂一样，当我们创建对象的时候，只要进行配置，完全可以不用管这些对象是怎么被创建出来的。

DI是实现控制反转的一种方法

## AOP

Aspect-Oriented Programming为业务模块所共同调用的逻辑或责任封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

Spring AOP是基于动态代理的，如果代理的对象实现了某个接口，那么Spring AOP会使用JDK Proxy去创建代理对象，如果没有实现结构对象，则使用Cglib来创建代理。当然也可以使用AspectJ。

### Spring AOP和AspectJ AOP的区别

1. Spring AOP是运行时增加，而AspectJ是编译时增加
2. Spring AOP基于代理，AspectJ基于字节码

Spring AOP已经集成了AspectJ。如果功能切面比较少，两者差异不大；如果切面过多，最好选择AspectJ。

## Spring bean

Spring使用工厂模式可以通过`BeanFactory`或`ApplicationContext`创建bean对象

### 作用域

- singleton：唯一bean实例，Spring中的bean默认都是单例的。并且对于所有的bean请求，只要id与该bean定义相匹配，则只会返回bean的同一实例。
- prototype：每次请求都会创建新的bean。prototype在创建容易的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且每次获取的都不是同一个对象。Spring并不对prototype bean生命周期负责，在装配一个bean后，就将它交给了客户端，清除prototype作用域对象的并释放任何prototype bean所持有的资源，都是客户端代码的职责。
- request：每次请求都会创建新的bean，且只在当前HTTP request中有效
- session：每次请求都会创建新的bean，且仅在当前HTTP session中有效
- global-session：全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5删除了protlet

对于有状态的bean应该使用prototype，对于无状态的bean应该使用singleton。

### singleton的bean线程安全问题

singleton bean存在线程安全问题，因为当多个线程操作同一个对象的时候，对这个对象的非静态成员变量的写操作会存在线程安全问题。

解决办法：

1. 不太现实的解决办法是不要唉bean中定义成员变量
2. 在类中定义一个`ThreadLocal`成员变量，将需要的成员变量保存在`ThreadLocal`中

Spring的单例是基于BeanFactory，单例bean在此容器中只有一个，Java单例是基于JVM，每个JVM内只有一个实例。

### `@Componet`和`@Bean`的区别

1. `@Componet`作用于类，而`@Bean`作用于方法
2. `@Componet`通常是通过扫描来自动侦测以及自动装配到容器中的，`@Bean`指示一个方法生成一个由Spring管理的bean
3. `@Bean`注解比`@Component`注解的自定义性更强

### 生命周期

大体可总结为：`装载 —> 装载后处理 —> 初始化 —> 初始化后处理 —> 销毁`

- Bean容器找到配置文件中 Spring Bean 的定义。
- Bean容器利用 Java Reflection API 创建一个Bean的实例。
- 如果涉及到一些属性值利用`set()`方法设置一些属性值。
- 如果Bean实现了`BeanNameAware`接口，调用`setBeanName()`方法，传入Bean的名字。
- 如果Bean实现了`BeanClassLoaderAware` 接口，调用`setBeanClassLoader()`方法，传入 `ClassLoader`对象的实例。
- 与上面的类似，如果实现了其他`*.Aware`接口，就调用相应的方法。
- 如果有和加载这个Bean的Spring容器相关的`BeanPostProcessor`对象，执行`postProcessBeforeInitialization()` 方法
- 如果Bean实现了`InitializingBean`接口，执行`afterPropertiesSet()`方法。
- 如果Bean在配置文件中的定义包含`init-method`属性，执行指定的方法。
- 如果有和加载这个Bean的 Spring 容器相关的 `BeanPostProcessor` 对象，执行`postProcessAfterInitialization()` 方法
- 当要销毁Bean的时候，如果Bean实现了 `DisposableBean`接口，执行`destroy()`方法。
- 当要销毁Bean的时候，如果Bean在配置文件中的定义包含destroy-method属性，执行指定的方法。

### `BeanFactory`和`ApplicationContext`对比

- `BeanFactory`：延迟注入
- `ApplicationContext`：容器启动的时候，无论是否用到，一次性创建所有bean，其有三个实现类：
    1. ClassPathXmlApplication
    2. FileSystemXmlApplication
    3. XmlWebApplicationContext

## Spring MVC

### `DispatcherServlet`工作原理

1. 客户端发送请求，请求直接到`DispatcherSeverlet`
2. `DispatcherSeverlet`根据请求信息调用`HandlerMapping`，解析对应的`Handler`
3. 开始由的`HandlerApdater`处理，`HandlerAdapter`会根据`Handler`调用真正的处理器开始处理请求，并处理相应业务逻辑
4. 处理器完成业务后，会返回一个`ModelAndView`对象
5. `ViewResolver`会根据逻辑`View`查找实际的`View`
6. `DispatcherServlet`会把返回的`Model`传给`View`进行渲染
7. 把`View`返回给请求者

### 重要组件

1. `DispatcherServlet`：Spring MVC的入口函数
2. HandlerMapping
3. HandlerAdapter：按照特定规则通过HandlerApdapter执行Handler
4. Handler：需要开发，处理具体的用户请求和业务逻辑
5. View Resolver：根据逻辑视图名解析成真正的视图
6. View：需要开发，是一个接口，实现类支持不同的View类型，比如JSP，Freemarker

## Spring Transaction

### 隔离级别

- `TransactionDefinition.ISOLATION_DEFAULT`：使用数据库默认的隔离级别
- `TransactionDefinition.ISOLATION_READ_UNCOMMITTED`:读未提交
- `TransactionDefinition.ISOLATION_READ_COMMITTED`:读已提交
- `TransactionDefinition.ISOLATION_REPEATABLE_READ`：可重复读
- `TransactionDefinition.ISOLATION_SERIALIZABLE`：串行化

### 传播行为

**支持当前事务的情况：**

- **`TransactionDefinition.PROPAGATION_REQUIRED`：**如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
- **`TransactionDefinition.PROPAGATION_SUPPORTS`：**如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
- **`TransactionDefinition.PROPAGATION_MANDATORY`：**如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。（mandatory：强制性）

**不支持当前事务的情况：**

- **`TransactionDefinition.PROPAGATION_REQUIRES_NEW`：**创建一个新的事务，如果当前存在事务，则把当前事务挂起。
- **`TransactionDefinition.PROPAGATION_NOT_SUPPORTED`：**以非事务方式运行，如果当前存在事务，则把当前事务挂起。
- **`TransactionDefinition.PROPAGATION_NEVER`：**以非事务方式运行，如果当前存在事务，则抛出异常。

**其他情况：**

- **`TransactionDefinition.PROPAGATION_NESTED`：** 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于`TransactionDefinition.PROPAGATION_REQUIRED`。

### `@Transactional`

当作用于类上时，该类的所有public方法都将具有该类型的事务属性，当发生异常时，将会回滚。

在注解中不配置`rollbackFor`属性，只会在遇到`RuntimeException`时候才会回归，加上`rollbackFor=Exception.class`，可以让事务在遇到非运行时异常也会回滚

### 事务管理接口介绍

- PlatformTranscationManager：Spring事务策略的核心
- TranscationDefinition：事务定义信息
- TransactionStatus

# 执行顺序

构造方法 >>> `@Autowrid`和`@Value` >>> `@PostConstuct`

[Spring 日常使用汇总](https://www.notion.so/Spring-09844f139b2e4e6789655eedc76bccb4?pvs=21)