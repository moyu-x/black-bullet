## Fork-join、Map-reduce 模型及区别

分布式计算技术，目前主要分为四大模式，主要包括 MapReduce 、Stream、Actor 以及流水线。

其背后都是分治归并思想，两个东西最本质的区别就是Fork-join被设计在单个Java程序上进行工作，而Map-Reduce则是被明确的设计为在大型集群上使用。最本质的区别就是F-J不能被拓展，只能在单个虚拟机上进行运行。多个小任务虽然运行在不同的处理器上，但是可以相互通信，甚至一个现场可以窃取其他线程上的子任务。

Java8的stream api中的并行流也是基于F-J来进行实现的。

Fork/Join 是一个并行计算的框架，主要就是用来支持分治任务模型的，这个计算框架里的**Fork 对应的是分治任务模型里的任务分解，Join 对应的是结果合并**。Fork/Join 计算框架主要包含两部分，一部分是**分治任务的线程池 ForkJoinPool**，另一部分是**分治任务 ForkJoinTask**。

Ref：

- [Java并发编程— Fork/Join：单机版的MapReduce | 一代键客](https://zhangquan.me/2023/07/12/java-bing-fa-bian-cheng-fork-join-dan-ji-ban-de-mapreduce/#toc-heading-1)
- [分布式计算技术MapReduce 详细解读 - 知乎](https://zhuanlan.zhihu.com/p/99334647)

## JDK中写复制原理

如果有多个调用者同时请求相同资源（如内存或磁盘上的数据存储），他们会共同获取相同的指针指向相同的资源，直到某个调用者试图修改资源的内容时，系统才会真正复制一份专用副本（private copy）给该调用者，而其他调用者所见到的最初的资源仍然保持不变。这个过程对其他的调用者是透明的（transparently）

无论是Hashtable–>ConcurrentHashMap，还是说Vector–>CopyOnWriteArrayList。JUC下支持并发的容器与老一代的线程安全类相比，总结起来就是加锁粒度的问题。

Java中利用此思想的主要就是CopyOnWriteArrayList及CopyOnWriteArraySet

### CopyOnWriteArrayList

Copy-on-write是解决并发的的一种思路，指的是实行读写分离，如果执行的是写操作，则复制一个新集合，在新集合内添加或者删除元素。待一切修改完成之后，再将原集合的引用指向新的集合。好处就是，可以高并发地对COW进行读和遍历操作，而不需要加锁，因为当前集合不会添加任何元素。

CopyOnWriteArrayList的核心理念就是读写分离，写操作在一个复制的数组上进行，读操作还是在原始数组上进行，读写分离，互不影响。写操作需要加锁，防止并发写入时导致数据丢失。写操作结束之后需要让数组指针指向新的复制数组。

>这样就会导致其读的性高于synchronizedList，但是写的性能会低于synchronizedList

不管是foreach循环还是直接写Iterator来遍历，实际上都是使用Iterator遍历。CopyOnWriteArrayList的迭代器是COWIterator。迭代器所有的操作都基于snapshot数组，而snapshot是构造函数传递进来的array数组（相当于遍历的时候创建原有list的拷贝，在此上进行遍历）。

> 因为只有写的时候才加锁，所以遍历只能保证最终一致性，如果a线程遍历list，然后b线程进行修改，此时a遍历的只能是老的数据。
> 并且每次遍历的时候都会进行拷贝，如果读取多了还会导致大量的gc以及更加麻烦的oom

ref:
- [面试官：你知道写时复制(Copy-On-Write)在Java中是如何被应用的吗？ - 残城碎梦 - 博客园](https://www.cnblogs.com/xfeiyun/p/15686157.html)
- [16 CopyOnWrite | 深入浅出Java多线程](https://redspider.gitbook.io/concurrent/di-san-pian-jdk-gong-ju-pian/16)

## 线程池原理

Java 中的线程池是通过 Executor 框架实现的。该框架中用到了 Executor，Executors， ExecutorService，ThreadPoolExecutor 这几个类。

ThreadPoolExecutor类常用参数：

- corePoolSize ：常驻线程数量（核心）， 线程池的核心线程数
- maximumPoolSize 能容纳的最大线程数
- keepAliveTime 空闲线程存活时间
- unit 存活的时间单位
- workQueue 存放提交但未执行任务的队列
- threadFactory 创建线程的工厂类
- handler 等待队列满后的拒绝策略

ThreadPoolExecutor中提供了两种执行任务的方法：

1. void execute(Runnable command)
2. Future submit(Runnable task)

![[assets/bfe42fb23c5950b333bc4e33a8ca285d_MD5.jpeg]]

有几个注意点：

1. ThreadPoolExecutor相当于是非公平的，比如队列满了之后提交的Runnable可能会比正在排队的Runnable先执行。
2. 提交一个Runnable时，不管当前线程池中的线程是否空闲，只要数量小于核心线程数就会创建新线程。

ThreadPoolExecutor的运行状态有5种，分别为：

![[assets/4379a7ab07673c353df750a13a44e24f_MD5.jpeg]]


常见的workqueue的选择：

![[assets/297d1792ee581f42e423d4e3d3942277_MD5.jpeg]]

四中常见的聚集策略：

![[assets/006a8cecbc4d2cf7822b1454968d9526_MD5.jpeg]]



ref: 

- [Java线程池实现原理及其在美团业务中的实践 - 美团技术团队](https://tech.meituan.com/2020/04/02/java-pooling-pratice-in-meituan.html)
- [还不懂Java线程池实现原理，看这一篇文章就够了 - 一灯架构 - 博客园](https://www.cnblogs.com/yidengjiagou/p/16895061.html)
- [线程池ThreadPoolExecutor工作原理超详细解析线程池基础 线程池概述 一种线程使用模式。线程过多会带来调度 - 掘金](https://juejin.cn/post/7251104465729437751)

## 原子性、可见行、有序性

**原子性**：即一个操作或者多个操作，要么全部执行并且执行的过程不会被任何因素打断，要么就都不执行。Java只保证了基本数据类型的变量和赋值操作才是原子性的。要想在多线程环境下保证原子性，则可以通过锁、synchronized来确保。volatile是无法保证复合操作的原子性。

**可见性**：可见性是指当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即看得到修改的值。

**有序性**：即程序执行的顺序按照代码的先后顺序执行。但是其本身会收到指令重排序的影响，指令重排序不会影响单个线程的执行，但是会影响到线程并发执行的正确性

synchronized具有可见性, volatile具有可见性

ref:

- [并发编程三大特性——原子性、可见性、有序性 - Ye_yang - 博客园](https://www.cnblogs.com/yeyang/p/13576636.html)
- [Java-concurrency/07.三大性质总结：原子性、可见性以及有序性/三大性质总结：原子性、可见性以及有序性.md at master · CL0610/Java-concurrency](https://github.com/CL0610/Java-concurrency/blob/master/07.%E4%B8%89%E5%A4%A7%E6%80%A7%E8%B4%A8%E6%80%BB%E7%BB%93%EF%BC%9A%E5%8E%9F%E5%AD%90%E6%80%A7%E3%80%81%E5%8F%AF%E8%A7%81%E6%80%A7%E4%BB%A5%E5%8F%8A%E6%9C%89%E5%BA%8F%E6%80%A7/%E4%B8%89%E5%A4%A7%E6%80%A7%E8%B4%A8%E6%80%BB%E7%BB%93%EF%BC%9A%E5%8E%9F%E5%AD%90%E6%80%A7%E3%80%81%E5%8F%AF%E8%A7%81%E6%80%A7%E4%BB%A5%E5%8F%8A%E6%9C%89%E5%BA%8F%E6%80%A7.md)
## 内存可见性

内存可见性是指在多线程编程中，当一个线程修改了共享变量的值后，其他线程是否能够立即看到这个修改后的值。

这里主要有三个原因会造成内存可见性的问题：

1. 多级缓存
2. 指令的重排序
3. 内存屏障

### MESI 协议

MESI 其实对应的是缓存中缓存行（CPU 缓存存储数据的最小单位，大小为 64B）的四种状态。

1. 已修改 Modified (M)  
2. 独占 Exclusive (E)  
3. 共享 Shared (S)  
4. 无效 Invalid (I)  

### 内存屏障

1. Store Barrier(写屏障)
2. Load Barrier(读屏障)
3. Full Barrier（全能屏障）

ref:
- [并发基础理论：缓存可见性、MESI协议、内存屏障、JMM - 知乎](https://zhuanlan.zhihu.com/p/84500221)

## 线程通讯方式

![[assets/4dcf880112d18e6b0fc5cc6b36ca0398_MD5.jpeg]]

ref:

- [TechCPP/problems/线程间的通信方式有那些？.md at master · youngyangyang04/TechCPP](https://github.com/youngyangyang04/TechCPP/blob/master/problems/%E7%BA%BF%E7%A8%8B%E9%97%B4%E7%9A%84%E9%80%9A%E4%BF%A1%E6%96%B9%E5%BC%8F%E6%9C%89%E9%82%A3%E4%BA%9B%EF%BC%9F.md)


## 并行和并发区别

并发，指的是多个事情，在同一时间段内同时发生了。  
并行，指的是多个事情，在同一时间点上同时发生了。

并发的多个任务之间是互相抢占资源的。  
并行的多个任务之间是不互相抢占资源的、

只有在多CPU的情况中，才会发生并行。否则，看似同时发生的事情，其实都是并发执行的。

一个cpu情况下发生的并行，是cpu轮训切换多个线程进行的

![[assets/94b8832e3ae1d14261693cfba1081683_MD5.jpeg]]


## Java 的四个引用类型

Java 引用是 Java 虚拟机为了实现更加灵活的对象生命周期管理而设计的对象包装类，一共有四种引用类型，分别是强引用、软引用、弱引用和虚引用

**强引用（Strong Reference）**：只要有引用存在，那么堆中数据不会被回收，哪怕是 OOM。普通申明和创建对象就是强引用

**软引用（SoftReference）**：在分配堆内存的时候，如果空间不足，就会将堆中的软引用的数据空间回收。`SoftReference softReference = new SoftReference(new byte[1024 * 1024 * 30]);//30M`
- 当发生 GC 时，虚拟机可能会回收 SoftReference 对象所指向的软引用，如果空间足够，就不会回收，还要取决于该软引用是否是新创建或近期使用过
- 在虚拟机抛出 OutOfMemoryError 之前，所有软引用对象都会被回收。

**弱引用（WeakReference）**:只要触发了 gc，无论内存空间是否充足，都会将堆中的弱引用的数据空间回收。`WeakReference weakReference = new WeakReference(new byte[1024 * 1024 * 5]);`
> 实际的使用就是ThreadLocal，他的Entity就是继承自WeakRefrence，原因是如果不使用这种方式，treadlocal的引用变量就会一直存在，有内存泄露的风险

**虚引用**是使用 PhantomReference 创建的引用，虚引用只有一个含有队列的构造函数，也就是说，虚引用必须和队列同时使用，换句话说，虚引用是通过队列来实现它的价值的。

ref:
- [JVM 系列（5）吊打面试官：说一下 Java 的四种引用类型 - 彭旭锐 - 博客园](https://www.cnblogs.com/pengxurui/p/16576791.html#:~:text=Java%20%E5%BC%95%E7%94%A8%E6%98%AFJava%20%E8%99%9A%E6%8B%9F,%E6%98%AF%E5%BC%B1%E5%8F%AF%E8%BE%BE%E7%9A%84%E3%80%82)
- [java的4种引用类型及应用场景_4种引用类型实例-CSDN博客](https://blog.csdn.net/hohoo1990/article/details/106356145)

## Map、Lis、Set、HashTable 技术原理及区别

List: 链表，主要有两种实现方式：

- ArrayList：本身底层使用数组进行数据存储，初始容量是16，如果指定，则是2n+1个大小，达到扩容的时候则是先创建原有数据两倍大小的新数组，然后进行数据拷贝，最后加入数据。所以最好初始化的时候就指定好数组大小
- LinkedList：底层是用链表进行实现存储，增加元素直接就在末尾指向下个指针就行
Set：集合，只保留唯一的元素
- HashSet：底层其实是使用hashmp来进行实现的，添加元素其实是讲增加的元素当作键存进去的
Map：键值对
* [[HashMap]]：线程不安全，由于其hashcode的实现方式，可以存储null键和null值，在接近临界点时，若此时两个或者多个线程进行put操作，都会进行resize（扩容）和reHash（为key重新计算所在位置），而reHash在并发的情况下可能会形成`链表环`
* ConcurrentHashMap: 在底层采用分段数组+链表的形式实现，保证线程安全，锁只会加在当前分段的数组上。读不加锁。entry是voliate的，所以保证可见性。扩容只会对当前的分段进行扩容
HashTable：使用数组加链表实现（类似于开链法解决哈希冲突），线程安全，因为在每个操作的时候都加了锁，初始的size为11，扩容的的大小为2n+1

ref：
- [HashMap、Hashtable、ConcurrentHashMap的原理与区别 - gqzdev - 博客园](https://www.cnblogs.com/gqzdev/p/hashmap.html)
- [【集合】List、Set、Map区别？ArrayList、HashMap、ConcurrentHashMap原理与区别 - 掘金](https://juejin.cn/post/7173896660094255117#heading-7)
## volatile

被volatile修饰的共享变量，就具有了以下两点特性：

- 保证了不同线程对该变量操作的内存可见性
- 禁止指令重排序；

volatile修饰的对象本身是原子的，但是修改其的方法不一定是原子的，比如`count++`这种

ref：
- [Java面试官最爱问的volatile关键字在Java的面试当中，面试官最爱问的就是volatile关键字相关的问题。经 - 掘金](https://juejin.cn/post/6844903989998288910)
## Happens-before

happens-before 指的是 Java 内存模型中两项操作的顺序关系。例如说操作 A 先于操作 B，也就是说操作 A 发生在操作 B 之前，操作 A 产生的影响能够被操作 B 观察到。

## Java的java.util.atomic包

见名知意，这是一个用于多线程使用的包，Atomic原子的意思，其本身就代表着这个包下的类的所有操作都是最微小不可分割的，所以也就需要保证对外的可见性。

包含原子操作的基本数据类型、原子操作的数组类型、原子操作的引用类型

其本身调用了unsafe类的cas操作，比如`unsafe.getAndAddInt`，在每次进行修改的时候，就会使用`for(;;)`自旋（这个自旋锁是导致性能的问题），然后调用cas类，直到操作成功

在Java8中有`LongAdder`上，使用了一个Cell数组来存储多个线程的修改情况，然后再惊醒CAS操作，当CAS更新失败的时候，就会尝试获取其他cell上处理的资源锁，减少了等待时间。用空间换时间

ref:
- [Java atomic包中的原子操作类（AtomicInteger）总结 | 二哥的Java进阶之路](https://javabetter.cn/thread/atomic.html#%E5%8E%9F%E5%AD%90%E6%9B%B4%E6%96%B0%E5%AD%97%E6%AE%B5%E7%B1%BB%E5%9E%8B)
- [面试必备：Java JUC LongAdder 详解[精品长文]LongAdder是JDK 1.8 新增的原子类，基于S - 掘金](https://juejin.cn/post/6844903909127880717)
- [AtomicXXX 用的好好的，阿里为什么推荐使用 LongAdder？面试必问！-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1952762)

## synchronized、reentrantLock区别

1. 用法不同：用法不同synchronized修饰代码快，reentrantLock则是先要构造对象，然后才能使用lock方法
2. 获取锁和释放锁的方式不同：synchronized会自动加锁和释放锁，而reentrantLock的这些操作需要手动编写，释放锁最好放在final中进行
3. 锁类型不同：synchronized是非公平锁，reentrantLock可以是公平锁，也可以是非公平锁，默认非公平锁
4. 响应中断不同：synchronized不能响应系统中断，会一直等待下去，可能会发生死锁。reentrantLock可以响应中断并释放
5. 实现不同：[[synchronized]]是通过监视器实现的，ReentrantLock是通过AQS(AbstractQueuedSynchronizer)进行实现的

ref:
- [Synchronize和ReentrantLock区别死锁的概念和产生死锁的根本原因是什么？死锁的预防策略中资源有序分配 - 掘金](https://juejin.cn/post/6844903695298068487)
- [面试突击：synchronized和ReentrantLock有什么区别？-51CTO.COM](https://www.51cto.com/article/707239.html)

## CountDownLatch和CyclicBarrier以及Semaphore

CountDownLatch和CyclicBarrier都能够实现线程之间的等待，只不过它们侧重点不同：

- CountDownLatch 一般用于某个线程A等待若干个其他线程执行完任务之后，它才执行；
- CyclicBarrier 一般用于一组线程互相等待至某个状态，然后这一组线程再同时执行；
- CountDownLatch 是不能够重用的，而 CyclicBarrier 是可以重用的。

countDownLatch可以实现类似计数器的功能（虽然也能模拟并发，但是不能达到同一时刻所有线程执行同一操作），CyclicBarrier则可以实现压测时候的并发请求

Semaphore 可以同时让多个线程同时访问共享资源，通过 acquire() 获取一个许可，如果没有就等待，而 release() 释放一个许可

ref:

- [CountDownLatch、CyclicBarrier、Semaphore的用法和区别CountDownLatch和C - 掘金](https://juejin.cn/post/7012864022282403847)
## AbstractQueuedSynchronizer

本质是一个队列（Queue），其内部维护着FIFO的双向队列，也就是CLH队列

CLH 锁通过引入一个队列来组织并发竞争的线程，对自旋锁进行了改进：

- 每个线程会作为一个节点加入到队列中，并通过自旋监控前一个线程节点的状态，而不是直接竞争共享变量。
- 线程按顺序排队，确保公平性，从而避免了 “饥饿” 问题。

AQS（AbstractQueuedSynchronizer）在 CLH 锁的基础上进一步优化，形成了其内部的 **CLH 队列变体**。主要改进点有以下两方面：

1. **自旋 + 阻塞**： CLH 锁使用纯自旋方式等待锁的释放，但大量的自旋操作会占用过多的 CPU 资源。AQS 引入了 **自旋 + 阻塞** 的混合机制：
    - 如果线程获取锁失败，会先短暂自旋尝试获取锁；
    - 如果仍然失败，则线程会进入阻塞状态，等待被唤醒，从而减少 CPU 的浪费。
2. **单向队列改为双向队列**


AQS实现了两套加锁解锁的方式，那就是**独占式**和**共享式**

非公平锁的实现仅仅是多了一个步骤：通过CAS的方式(compareAndSetState)尝试改变state的状态，修改成功后设置当前线程以独占的方式获取了锁，修改失败执行的逻辑和公平锁一样。

![[assets/b205d2e492ba3552135a923ecec99661_MD5.jpeg]]

ref:
- [面试必问的AQS（AbstractQueuedSynchronizer），一次性全搞定！ - 知乎](https://zhuanlan.zhihu.com/p/178991617)
- [AQS 详解 | JavaGuide](https://javaguide.cn/java/concurrent/aqs.html#aqs-%E8%B5%84%E6%BA%90%E8%8E%B7%E5%8F%96%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90-%E7%8B%AC%E5%8D%A0%E6%A8%A1%E5%BC%8F)

## JVM、内存模型和性能调优

JVM(Java Virtual Machine)：是一种抽象的计算机，基于堆栈架构，它有自己的指令集和内存管理。它加载 class 文件，分析、解释并执行字节码。JVM 主要分为三个子系统：**类加载器**、**运行时数据区**和**执行引擎**。

### 类加载器

- 类加载过程：**加载->连接->初始化**。
- 连接过程又可分为三步：**验证->准备->解析**。


JVM 启动的时候，并不会一次性加载所有的类，而是根据需要去动态加载。对于已经加载的类会被放在 `ClassLoader` 中。

JVM 中内置了三个重要的 `ClassLoader`：

1. **`BootstrapClassLoader`(启动类加载器)**
2. **`ExtensionClassLoader`(扩展类加载器)**
3. **`AppClassLoader`(应用程序类加载器)**

### 双亲委派模型

- 在类加载的时候，系统会首先判断当前类是否被加载过。已经被加载的类会直接返回，否则才会尝试加载（每个父类加载器都会走一遍这个流程）。
- 类加载器在进行类加载的时候，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成（调用父加载器 `loadClass()`方法来加载类）。这样的话，所有的请求最终都会传送到顶层的启动类加载器 `BootstrapClassLoader` 中。
- 只有当父加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需的类）时，子加载器才会尝试自己去加载（调用自己的 `findClass()` 方法来加载类）。
- 如果子类加载器也无法加载这个类，那么它会抛出一个 `ClassNotFoundException` 异常。
双亲委派模型保证了 Java 程序的稳定运行，可以避免类的重复加载（JVM 区分不同类的方式不仅仅根据类名，相同的类文件被不同的类加载器加载产生的是两个不同的类），也保证了 Java 的核心 API 不被篡改。

### 内存区域

**线程私有的：**

- 程序计数器
- 虚拟机栈
- 本地方法栈

**线程共享的：**

- 堆
- 方法区
- 直接内存 (非运行时数据区的一部分)

其他：
- 常量池

---

JMM（java内存模型）：抽象了线程和主内存之间的关系，就比如说线程之间的共享变量必须存储在主内存中。

![[assets/22b3bc5411c80d6aa8c62c328eb63a7a_MD5.jpeg]]

### 性能调优

性能调优的首要是要解决是否需要调优，就算最终经过监控测试发现调优的话，也是首先在代码层面上进行调优，大部分公司的大部分场景直接使用jvm参数进行调优，不会有质的变化。并且大部分时候通过jprofile和eclipse的mat看下来，很多都是代码编写引起的。

调优的目标：

- 延迟：GC低停顿和GC低频率；
- 低内存占用；
- 高吞吐量;
## OutOfMemoryError和StackOverFlowError

StackOverFlowError：每一个 JVM 线程都拥有一个私有的 JVM 线程栈，用于存放当前线程的 JVM 栈帧（包括被调用函数的参数、局部变量和返回地址等）。如果某个线程的线程栈空间被耗尽，没有足够资源分配给新创建的栈帧，比如递归的深度过高

OutOfMemoryError：内存溢出

- 栈内存溢出
- 堆内存溢出
- 方法区内存溢出