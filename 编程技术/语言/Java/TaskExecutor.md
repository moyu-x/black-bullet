## 开启异步

当再`Spring boot`程序中需要实用异步的时候，可以使用`@EnableAsync`注解来开启异步执行支持，然后在需要实用异步的地方可以实用`@Async`使用。

## 自定义配置

默认的情况下，Spring 将搜索所有和线程池关联的定义：在上线文中唯一的`org.springframework.core.task.TaskExecutor`的`bean`或者名称叫做`taskExecutor`的`java.util.concurrent.Executor`的`bean`。当这些都没有发现的时候， 就会实用`org.springframework.core.task.SimpleAsyncTaskExecutor`，原注释建议自己实现一个配置。

![[assets/6231df5af47a724b6a9c5548a4cb4795_MD5.jpg]]

当通过实现`AsyncConfigurer`的配置需要注意：

![[assets/a03a789bc9ba36c7c29113e4b18f3d9b_MD5.jpg]]

当只有一个属性需要配置的时候继承`AsyncConfigurerSupport`来实现

当配置是`@EnableAsync(proxyTargetClass = true)`的时候，所有 Spring 进行管理的`bean`都需要代理。

![[assets/c946cb8e27776f5b6dbe78ad3d934bee_MD5.jpg]]

## 线程池达到最大时的拒绝策略

1. `CallerRunsPolicy()`不在新线程中执行任务，而是调用任务的`run()`方法绕过线程池直接执行
2. `AbortPolicy()`拒绝执行新的任务，并且抛出`RejectedExecutionException`异常，这个是默认值
3. `DiscardPolicy()`直接取消新的任务，但是不抛出异常，不推荐使用
4. `DiscardOldestPolicy()`丢弃最久未执行的任务，让后把当前任务加入队列中