# Spring Data
## Redis 使用 Redisson 的版本问题

ref：[redisson/redisson-spring-boot-starter/README.md at master · redisson/redisson](https://github.com/redisson/redisson/blob/master/redisson-spring-boot-starter/README.md)

需要在`redisson-spring-boot-starter`中排除不合适的版本，这样才能稳定的引入

## Gradle

### Gradle 使用阿里镜像

Gradle 使用阿里镜像在`build.gradle`文件中加入如下配置：

```
repositories {
    maven {url '<http://maven.aliyun.com/nexus/content/groups/public/>'}
}
```

### Gradle 打包到本地仓库

在`build.gradle`的文件种添加如下代码：

```
plugins {
    id 'maven'
    id 'maven-publish'
}

// 开启 jar 打包支持
jar {
  enabled = true
}
```

当需要使用自定义`pom`文件的时候，可以参考[MavenPublish](https://docs.gradle.org/current/dsl/org.gradle.api.publish.maven.MavenPublication.html)的文档，当需要使用的时候，在`repositories`种加入`mavenLocal()`即可

## 对`List`中的数据进行验证

如果只是要求`List`中必须有值，使用`@Min`注解就行，如果还需要对其中的元素进行验证，还需要使用`@Valid`进行验证传递

## Feign

### Feign From 的使用

建立一个`feign`的配置文件，并在其中写入如下代码：

```kotlin
@Bean
fun encoder(): Encoder {    
     return FormEncoder()
}
```

然后在请求到接口上加入`consumes = [MediaType.APPLICATION_FORM_URLENCODED_VALUE]`，对于请求到参数，使用`model`进行封装，然后使用`map`封装请求参数

### feign 在 spring boot 2.0 中提示错误

当使用 `Spring boot 2.0` 的时候，idea 会提示无法注入，这个时候在`client`中加入`@Component`注解就可以了

### feign 的 name 同名提示报错

当在`Spring boot 2.0`使用`feign`的时候，使用`Bean`提示如下错误：

```
Description:

The bean Bean 的名称，defined in null, could not be registered. A bean with that name has already been defined in null and overriding is disabled.

Action:

Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=tru
```

此时，可以考虑如下两个解决办法：

1. 在配置文件中加入运行使用同名`Bean`的配置

```
spring:  main:    allow-bean-definition-overriding: true
```

1. 将每个`Bean`配置成不同的名称

### Feign 中 LocalDate 错误

在`Feign`使用`LocalDate`调用其他服务的接口时候，对方接口也是使用`LocalDate`进行接收的时候会报如下错误：

![[assets/67c9a23ee57d4892edf99c9f86c19536_MD5.jpg]]

image.png

这是因为两边的时间格式不一样，这个时候可以 Feign 上用格式化后的`String`对象传出

## 兼容问题

### 自动注入冲突

在使用`Spring boot 2.0`的时候，有可能会出现多个自动配置到`Bean`冲突到情况，这个时候可以将不需要注入到`Bean`加入到`spring.autoconfigure.exclude`中就可以了，例如`consul`和`kubernetes`冲突到情况，可以用如下设置：

```
spring:  autoconfigure:    exclude:      - org.springframework.cloud.kubernetes.discovery.KubernetesDiscoveryClientAutoConfiguratio
```

### lombok 在 IDEA 中提示报错

当出现 lombok 报错的时候，在 `peferences -> build -> annotation processors`中设置

### Java 和 Kotlin 类型不兼容

在使用 spring data redis 允许 lua 脚本的时候会出现返回值是 Int，然后在 kotlin 中出现了类型不兼容的情况，这是因为 kotlin 中的 Long 指向的是 Java 中的 long，并且在接口上做了不可以为空判定，大部分情况下是可以兼容的，但是 spring 框架类中使用了反射强制获取了 Java 中的 Long 类型，也就是在获取包装类型的情况下，只能强制指定返回的类型为包装类习惯，这是因为在不指定的话默认的类型转换是不可空的，如下

```
java.lang.Longjava.lang.Long::class.java
```

### Consul 和 Kubernetes 同时使用时候冲突的解决

`Spring boot`的一个功能就是提供了大量的自动配置，当多种同类型的自动配置进行加载的时候，就会出现冲突，继而导致应用不能启动，这个时候就需要在系统中根据不同的环境加载不同的配置文件，`Spring boot`提供了不加载某些自动配置的配置项，下面以不加载`Consul`，而使用`Kubernetes`为例：

```
spring:  autoconfigure:    exclude:      - org.springframework.cloud.consul.serviceregistry.ConsulAutoServiceRegistrationAutoConfiguration
```

## 简单的分布式锁的实现

实现分布式锁最简单的方式就是在 reids 中执行 lua 脚本，因为 lua 是单进程执行的，在不是集群部署的情况下实现一个分布式锁或者进行分布式限流相对来说是比较简单的。当在系统部署的时候使用了集群的方式，应该考虑的是数据在每个 slot 中同步的问题，一个简单的解决办法是在少数节点只进行写入操作，在多数节点进行写操作。在 spring 中，使用 spring data redis 对大部分的 redis 操作进行了封装，并且可以在使用集群的情况下设置写入节点和读取节点，然后在其中对数据进行处理，所有的后续操作都要等到 redis 中的数据真实有效变更后才能进行后续操作

## Spring boot 2 Actuator 显示所有 bean

在配置文件中加如如下配置：

```
management:  endpoint:    beans:      enabled: true  endpoints:    web:      exposure:        include: "*"
```

## Spring boot 1.x 中分页数量大于 2000

使用`@PageDefault`去进行分页请求处理的时候，当`page_size`超过 2000 的时候会默认 2000 的分页的请求，源码中的默认上限是 1000，最高能到 2000 的分页请求，在使用 Spring boot2 的及以上的时候直接修改配置文件中的相关配置就行，如果使用的是之前的版本，使用如下的代码：

```java
@Configuration 
@EnableConfigurationPropertiespublic 
class PaginationConfiguration extends SpringDataWebConfiguration {
    @Bean public PageableHandlerMethodArgumentResolver pageableResolver() {
        PageableHandlerMethodArgumentResolver pageableHandlerMethodArgumentResolver = new PageableHandlerMethodArgumentResolver(
            sortResolver());
        pageableHandlerMethodArgumentResolver.setMaxPageSize(Integer.MAX_VALUE);
        return pageableHandlerMethodArgumentResolver;
    }
}
```

主要原因是在官方在实现的时候认为一个分页超过 2000 没有意义，并且有可能有数据问题，在我们的项目中虽然使用了微服务的方式，但是拥有一个 web 层在其之上，所有的导出数据基本都是在 web 层上完成的，所以才会有此类的情况。更为好的方式是使用 DDD 的方式取构建项目，这个时候就不需要在内网中拉取全部的数据，而是在数据库中取出相关的数据就就行了，和分页也就没有任何关系了。

## Consul

### 高可用 config-server

当使用 consul 注册多个 config-server 实例实现高可用的时候，任何和 consul 相关的配置文件都应该写到`bootstrap.yml`中，不能写到 config-server 中，因为启动的时候会先去连接 consul 才会去读 config-server

# Redis

## 自定义Cache注解后出现java.lang.CastException

这个主要是默认的序列化（JdkSerialization）会导致虽然包名一致，但是会被认为是两个不同的类加载器加载的数据。

解决这种情况可以可以让JdkSerialization直接去获取最终的类加载器，这样能解决，或者直接在redis中使用json的序列化方式。

```java
    @Bean
    @Override
    public CacheManager cacheManager() {
    JdkSerializationRedisSerializer redisSerializer = new JdkSerializationRedisSerializer(getClass().getClassLoader());
    return new PrsRedisCacheManager(
            RedisCacheWriter.nonLockingRedisCacheWriter(redisConnectionFactory()),
            RedisCacheConfiguration.defaultCacheConfig().serializeValuesWith(
                    RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer)
            ));
    }
```

# MyBatis

## 查看日志

在Spring boot中查看MyBatis的SQL日志，可以直接在配置文件中将包路径写入，并配置debug或trace即可：

```yaml
logging:
  level:
    top.idwangmo.mybatisdemo: trace
```

## ID的生成

在主键生成的时候，mybatis-plus会自动插入一个id到实体对象中，并且看结构这个是基于sonwflake算法实现的。如果使用integer字段的时候会导致提示数据太大了

### 修改字段类型

可以修改字段类型来解决这个问题，比如更改字段类型为long，或者由7位的integer更新成20位的

### 修改id的注解

```java
@TableId(value = "id",type = IdType.AUTO)
```

## 在静态类中使用@value进行注入

在静态工具类中使用自动注入功能首先得在类上加入`@Component`注解，剩下有两种方式可以实现在静态方法中使用：

1. 使用`@PostConstruct`有两种方式，然后在其中对数据进行初始化
    
    ```java
    @Component
    public ExampleUtil {
        @Value("{example}")
        private String example;
        private static String exampleStatic;
    
        @PostConstrict
        public void init() {
            exampleStatic = this.example;
        }
    }
    ```
    
2. 使用Setter注入的方式进行使用
    
    ```java
    @Component
    public ExampleUtil {
    
        private static String exampleStatic;
    
        @Value("{example}")
        public void setExampleStatic(String example) {
            exampleStatic = example;
        }
    }
    ```
    
    ## Kafka
    
    ### 提示主机名需要校验
    
    在kafka-client 2 版本中，会去校验证书的主机名，所以需要忽略主机名称：
    
    ```yaml
    sprinspring:
      kafka:
        properties:
          ssl:
            endpoint:
              identification:
                algorithm: ''
    ```