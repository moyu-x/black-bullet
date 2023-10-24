[[JVM]]
[[JMM]]
[ThreadLocal](Java/ThreadLocal.md)

---
- 基本类型
	- [[String]]
- [[多线程]]
- [[Java文章收集]]
- JVM
	- 类加载机制
		- [[双亲委派]]
		- 默认的类加载器
			- Bootstrap ClassLoader
				- 加载jdk自带的`rt.jar`中的类文件
				- 是所有加载器的父类
			- Extension ClassLoader
				- 负责加载Java的扩展类库从`jre/lib/ect`或`java.ext.dirs`类系统属性指定的目录下的类
			- System ClassLoader (Application ClassLoader)
				- 负责从classpath环境变量中加载类文件
- Java Agent
	- 用处
		- 代理：获取一些JVM的运行指标
		- 探针：对于黑盒的JVM可以深入底部了解数据
	- 启动方式
		- 以 JVM 启动参数 -javaagent:xxx.jar 的形式随着 JVM 一起启动，这种情况下，会调用 premain 方法，并且是在主进程的 main 方法之前执行
		- 以 loadAgent 方法动态 attach 到目标 JVM 上，这种情况下，会执行 agentmain 方法
	- 实现agentmain和premain两个方法