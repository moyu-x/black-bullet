# Java

- [Java](about:blank#java)
    - [生态](about:blank#%E7%94%9F%E6%80%81)
    - [基本规则](about:blank#%E5%9F%BA%E6%9C%AC%E8%A7%84%E5%88%99)
        - [HashCode 和 Equals 的重写](about:blank#hashcode-%E5%92%8C-equals-%E7%9A%84%E9%87%8D%E5%86%99)
        - [CompareTo](about:blank#compareto)
        - [提高类型拷贝等 IO 操作的性能](about:blank#%E6%8F%90%E9%AB%98%E7%B1%BB%E5%9E%8B%E6%8B%B7%E8%B4%9D%E7%AD%89-io-%E6%93%8D%E4%BD%9C%E7%9A%84%E6%80%A7%E8%83%BD)
    - [抽象类和接口](about:blank#%E6%8A%BD%E8%B1%A1%E7%B1%BB%E5%92%8C%E6%8E%A5%E5%8F%A3)
        - [基准测试需要注意的问题](about:blank#%E5%9F%BA%E5%87%86%E6%B5%8B%E8%AF%95%E9%9C%80%E8%A6%81%E6%B3%A8%E6%84%8F%E7%9A%84%E9%97%AE%E9%A2%98)
    - [Error 和 Exception](about:blank#error-%E5%92%8C-exception)
        - [理解](about:blank#%E7%90%86%E8%A7%A3)
        - [处理](about:blank#%E5%A4%84%E7%90%86)
        - [常见的 Error 和 Exception](about:blank#%E5%B8%B8%E8%A7%81%E7%9A%84-error-%E5%92%8C-exception)
            - [NoClassDefFoundError](about:blank#noclassdeffounderror)
            - [ClassNotFoundException](about:blank#classnotfoundexception)
    - [final, finally, finalize 的区别](about:blank#final-finally-finalize-%E7%9A%84%E5%8C%BA%E5%88%AB)
    - [引用](about:blank#%E5%BC%95%E7%94%A8)
        - [分类](about:blank#%E5%88%86%E7%B1%BB)
        - [特点](about:blank#%E7%89%B9%E7%82%B9)
    - [Fork/Join 框架](about:blank#forkjoin-%E6%A1%86%E6%9E%B6)
        - [参考资料](about:blank#%E5%8F%82%E8%80%83%E8%B5%84%E6%96%99)
    - [常见类](about:blank#%E5%B8%B8%E8%A7%81%E7%B1%BB)
        - [String, StringBuffer, StringBuilder](about:blank#string-stringbuffer-stringbuilder)
    - [代理和反射](about:blank#%E4%BB%A3%E7%90%86%E5%92%8C%E5%8F%8D%E5%B0%84)
        - [动态代理](about:blank#%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86)
            - [反射](about:blank#%E5%8F%8D%E5%B0%84)
    - [集合](about:blank#%E9%9B%86%E5%90%88)
    - [List](about:blank#list)
        - [Map](about:blank#map)
            - [LinkedHashMap](about:blank#linkedhashmap)
            - [TreeMap](about:blank#treemap)
            - [HashMap](about:blank#hashmap)
            - [结构](about:blank#%E7%BB%93%E6%9E%84)
                - [负载因子](about:blank#%E8%B4%9F%E8%BD%BD%E5%9B%A0%E5%AD%90)
                - [为什么 HashMap 要树化呢](about:blank#%E4%B8%BA%E4%BB%80%E4%B9%88-hashmap-%E8%A6%81%E6%A0%91%E5%8C%96%E5%91%A2)
                - [解决哈希冲突的方法](about:blank#%E8%A7%A3%E5%86%B3%E5%93%88%E5%B8%8C%E5%86%B2%E7%AA%81%E7%9A%84%E6%96%B9%E6%B3%95)
            - [ConcurrentHashMap](about:blank#concurrenthashmap)
                - [实现](about:blank#%E5%AE%9E%E7%8E%B0)
    - [IO](about:blank#io)
        - [NIO](about:blank#nio)
        - [NIO Buffer](about:blank#nio-buffer)
    - [锁](about:blank#%E9%94%81)
        - [基础](about:blank#%E5%9F%BA%E7%A1%80)
        - [synchronized](about:blank#synchronized)
        - [ReentrantLock](about:blank#reentrantlock)
        - [Condition](about:blank#condition)
        - [自旋锁](about:blank#%E8%87%AA%E6%97%8B%E9%94%81)
    - [线程](about:blank#%E7%BA%BF%E7%A8%8B)
        - [状态](about:blank#%E7%8A%B6%E6%80%81)
        - [死锁](about:blank#%E6%AD%BB%E9%94%81)
        - [线程池](about:blank#%E7%BA%BF%E7%A8%8B%E6%B1%A0)
        - [CAS](about:blank#cas)
        - [AQS](about:blank#aqs)
            - [设计初衷](about:blank#%E8%AE%BE%E8%AE%A1%E5%88%9D%E8%A1%B7)
            - [基本结构](about:blank#%E5%9F%BA%E6%9C%AC%E7%BB%93%E6%9E%84)
    - [类加载](about:blank#%E7%B1%BB%E5%8A%A0%E8%BD%BD)
    - [类加载](about:blank#%E7%B1%BB%E5%8A%A0%E8%BD%BD-1)
        - [动态代理](about:blank#%E5%8A%A8%E6%80%81%E4%BB%A3%E7%90%86-1)
    - [内存](about:blank#%E5%86%85%E5%AD%98)
        - [JVM 内存区域](about:blank#jvm-%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F)
        - [OOM](about:blank#oom)
            - [OutOfMemoryEroor 的原因](about:blank#outofmemoryeroor-%E7%9A%84%E5%8E%9F%E5%9B%A0)
    - [垃圾回收](about:blank#%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6)
        - [常见的垃圾回收器](about:blank#%E5%B8%B8%E8%A7%81%E7%9A%84%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E5%99%A8)
        - [对象实例收集](about:blank#%E5%AF%B9%E8%B1%A1%E5%AE%9E%E4%BE%8B%E6%94%B6%E9%9B%86)
        - [算法](about:blank#%E7%AE%97%E6%B3%95)
        - [调优](about:blank#%E8%B0%83%E4%BC%98)
        - [JMM](about:blank#jmm)
        - [Docker](about:blank#docker)
    - [安全](about:blank#%E5%AE%89%E5%85%A8)
        - [组成](about:blank#%E7%BB%84%E6%88%90)

## 生态

![[assets/5f79c5f77af6f39f2a9e83e90e60ef86_MD5.jpg]]

生态

AOT （ Ahead-of-Time Compilation ），直接将字节码编译成机器代码，这样就避免了 面的开销，比如 Oracle JDK 9 就引入了实验性的 AOT 特性，并且增加了新的 jaotc 工具。

Java 的特性：

- 面向对象：封装、继承、多态
- 平台无关性
- 语言
- 类库

## 基本规则

### HashCode 和 Equals 的重写

- equals 相等，hashCode 一定要相等。
- 重写了 hashCode 也要重写 equals。
- hashCode 需要保持一致性，状态改变返回的哈希值仍然要一致。
- equals 的对称、反射、传递等特性。

### CompareTo

自然顺序同样需要符合一个约定，就是 `compareTo` 的返回值需要和`equals`一致，否则就会出现模棱两可情况

### 提高类型拷贝等 IO 操作的性能

- 在程序中，使用缓存等机制，合理减少 IO 次数
- 使用 transferTo 等机制，减少上下文切换和额外 IO 操作
- 尽量减少不必要的转换过程

## 抽象类和接口

接口是对象行为的抽象，它是抽象方法的集合，利用接口可以达到 API 定义和实现分离的目的。抽象类是不能实例化的类，用`abstract`关键字修饰`class`，其目的的主要是代码重用。

### 基准测试需要注意的问题

- 保证代码已经经过了足够且合适的预热
- 防止 JVM 进行无效代码消除
- 防止发生常量折叠
- 需要考虑方法内联对性能的影响

## Error 和 Exception

```
@startuml
Object <|-- Throwable
Throwable <|-- Error
Throwable <|-- Exception
Exception <|-- RuntimeException
Exception <|-- 非RuntimeException
@enduml
```

### 理解

`Exception`和`Error`体现了 Java 平台设计者对不同异常的分类：

- `Exception`是程序正常吃运行中，可以预料的意外情况，可能并且应该被捕获，进行相应处理
- `Error`是指在正常情况下，不太可能出现的情况，绝大部分的`Error`都会导致程序处于非正常、不可恢复的状态

可检查异常在源代码里必须显式地进行捕获处理，这是编译期检查的一部分。不检查异常就是运行时异常，通常是可以编码避免的逻辑错误，具体根据需要来判断是否需要捕 获，并不会在编译期强制要求。

### 处理

理解异常：

1. 理解 Throwable、Exception、Error 的设计和分类 。
2. 理解 Java 语言中操作 Throwable 的元素和实践 。

处理异常的基本原则：

1. 尽量不要捕获`Exception`这种通用异常，而是捕获特定异常，多个异常可以使用`multi-catch`进行统一处理
2. 不要在捕获异常后不对异常进行处理

自定义异常需要考虑的地方：

- 是否需要定义成`Checked Exception`，因为这种类型设计的初衷更是为了从异常情况恢复，作为异常设计者，我们往往有充足信息进行分类。
- 在保证诊断信息足够的同时，也要考虑避免包含敏感信息，因为那样可能导致潜在的安全问题

不要在`try`语句块中使用`return`，`break`和`continue`语句，因为其可能使代码进入`finally`语句块，万一无法避免，要确保`finally`的存在不会改变函数的返回值

Java 异常处理耗费性能的地方：

- `try-catch`代码段会产生额外的性能开销，或者换个角度说，它往往会影响 JVM 对代码进行优化，所以建议仅捕获有必要的代码段，尽量不要一个大的 try 包住整段的代码。
- Java 每实例化一个`Exception`，都会对当时的栈进行快照，这是一个相对比较重的操作。如果发生的非常频繁，这个开销可就不能被忽略了。

### 常见的 Error 和 Exception

### NoClassDefFoundError

`NoClassDefFoundError`产生的原因在于： 如果 JVM 或者 ClassLoader 实例尝试加载（可以通过正常的方法调用，也可能是使用 new 来创建新的对象）类的时候却找不到类的定义。要查找的类在编译的时候是存在的，运行的时候却找不 到了。这个时候就会导致`NoClassDefFoundError`.

### ClassNotFoundException

`ClassNotFoundException`的产生原因：Java 支持使用`Class.forName`方法来动态地加载类， 运行时抛出 ClassNotFoundException 异常。任意一个类的类名如果被作为参数传递给这个方法都将导致该类被加载到 JVM 内存中，如果这个类在类路径中没有被找到，那么此时就会在运行时抛出`ClassNotFoundException`异常。

## final, finally, finalize 的区别

fnalize 机制现在已经不推荐使用，并且在 JDK 9 开始被标记 为 `deprecated`,为无法保证其在什么时候进行执行，`finalize`会掩盖资源回收时候的出错信息

如果需要实现`immutable`类，需要做到：

- 将`class`类声明为`final`
- 将所有成员变量定义为`private`或`final`，并且不要实现`setter`方法
- 通常构造对象的时候，成员变量使用深度拷贝来初始化，而不是直接赋值
- 如果有需要实现`getter`方法或返回其内部状态的方法，使用`copy-on-write`原则

**资源用完即显示释放，或者利用资源池来尽量重用**

`fnal`变量产生了某种程度的不可变（immutable）的效果，所以，可以用于保护只读数据，尤其是在并发编程中，因为明确地不能再赋值 fnal 变量，有利于减少额外的同步开销，也可以省去一些防御性拷贝的必要。

Java 平台目前使用`java.lang.ref.Cleaner`来替换原有的`finalize`实现，`Cleaner`的实现利用了`PhantomReference`，这是一种常见的`post-morterm`清理机制

Java 正在使用`java.lang.ref.Cleaner`来替换`finalize`实现，这个代码在 1.8 中没有，在 11 有此代码

`fianlly`不会被执行的情况：

- `try-catch`异常退出
- 无限循环
- 线程被杀死

**不然最好不要指望这种小技巧带 来的所谓性能好处，程序最好是体现它的语义目的。**

## 引用

### 分类

- 强引用(Strong Reference)
- 软引用(Soft Reference)
- 弱引用(WeakReference)
- 幻象引用 (PhantomReference)——虚引用

**不同的引用类型，主要体现的是 对象不同的可达性（reachable）状态和对垃圾收集的影响 。****Java 定义的不同可达性级别（ reachability level ），具体如下：

- 强可达（Strongly Reachable），就是当一个对象可以有一个或多个线程可以不通过各种引用访问到的情况。比如，我们新创建一个对象，那么创建它的线程对它就是强可达。
- 软可达（Softly Reachable），就是当我们只能通过软引用才能访问到对象的状态。
- 弱可达（Weakly Reachable），类似前面提到的，就是无法通过强引用或者软引用访问，只能通过弱引用访问时的状态。这是十分临近 fnalize 状态的时机，当弱引用被清除的 时候，就符合 fnalize 的条件了。
- 幻象可达（Phantom Reachable），上面流程图已经很直观了，就是没有强、软、弱引用关联，并且 fnalize 过了，只有幻象引用指向这个对象的时候。
- 当然，还有一个最后的状态，就是不可达（unreachable），意味着对象可以被清除了。

对于软引用的垃圾回收倾向于各个`JVM`的实现

软引用通常会在最后一次引用后，还能保持一段时间，默认值是根据堆剩余空间计算的（以 M bytes 为单位）。这个剩余空间，其实会受不同 JVM 模式影响，对于 Client 模式，比如通常的 Windows 32 bit JDK，剩余空间是计算当前堆里空闲的大小，所以更加倾向于回收；而对于 server 模 式 JVM，则是根据 -Xmx 指定的最大值来计算。

### 特点

1. 强引用

特点：我们平常典型编码 Object obj = new Object()中的 obj 就是强引用。 通过关键字 new 创建的对象所关联的引用就是强引用。 当 JVM 内存空间不足，JVM 宁愿抛出 OutOfMemoryError 运 行时错误（OOM），使程序异常终止，也不会靠随意回收具有强引用的「存活」对象来解决内存不足的问题。对于一个普通的对象，如果没有其他的引用关系，只要超过了引用的作用域或者显式 地将相应（强）引用赋值为 null， 就是可以被垃圾收集的了，具体回收时机还是要看垃圾收集策略。

1. 软引用

特点：软引用通过 SoftReference 类实现。 软引用的生命周期比强引用短一些。只有当 JVM 认为内存不足时，才会去试图回收软引用指向的对象：即 JVM 会确保在抛出 OutOfMemoryError 之前，清理软引用指向的对象。软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收器回收，Java 虚拟机就会把这个软引用加入到与之关联的引用 队列中。后续，我们可以调用 ReferenceQueue 的 poll()方法来检查是否有它所关心的对象被回收。如果队列为空，将返回一个 null,否则该方法返回队列中前面的一个 Reference 对象。

应用场景：软引用通常用来实现内存敏感的缓存。如果还有空闲内存，就可以暂时保留缓存，当内存不足时清理掉，这样就保证了使用缓存的同时，不会耗尽内存。

1. 弱引用

弱引用通过 WeakReference 类实现。 弱引用的生命周期比软引用短。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了具有弱引用的对象，不管当前内存空间足够与否，都会 回收它的内存。由于垃圾回收器是一个优先级很低的线程，因此不一定会很快回收弱引用的对象。弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾 回收，Java 虚拟机就会把这个弱引用加入到与之关联的引用队列中。

应用场景：弱应用同样可用于内存敏感的缓存。

1. 虚引用

特点：虚引用也叫幻象引用，通过 PhantomReference 类来实现。无法通过虚引用访问对象的任何属性或函数。幻象引用仅仅是提供了一种确保对象被 fnalize 以后，做某些事情的机制。如果 一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收。虚引用必须和引用队列 （ReferenceQueue）联合使用。当垃圾回收器准备回收一个对象时，如 果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。ReferenceQueue queue = new ReferenceQueue (); PhantomReference pr = new PhantomReference (object, queue); 程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之 前采取一些程序行动。

应用场景：可用来跟踪对象被垃圾回收器回收的活动，当一个虚引用关联的对象被垃圾收集器回收之前会收到一条系统通知。

---

引用出现的根源是由于 GC 内存回收的基本原理—GC 回收内存本质上是回首对象，而目前比较流行的回收算法是可达性分析算法，从 GC Roots 开始按照一定的逻辑判断一个对象是否可达， 不可 达的话就说明这个对象已死（除此之外另外一种常见的算法就是引用计数法，但是这种算法有个问题就是不能解决相互引用的问题）。

## Fork/Join 框架

`fork`操作将会启动一个新的并行`fork/join`子任务。`join`操作会一直等待直到所有的子任务都结束。`fork/join`算法，如同其他分治算法一样，总是会递归的、反复的划分子任务，直到这些子任务可以用足够简单的、短小的顺序方法来执行。

`fork/join`程序可以在任何支持以下特性的框架之上运行：框架能够让构建的子任务并行执行，并且拥有一种等待子任务运行结束的机制。然而，`java.lang.Thread`类（同时也包括`POSIX pthread`，这些也是`Java`线程所基于的基础）对`fork/join`程序来说并不是最优的选择：

- `fork/join`任务对同步和管理有简单的和常规的需求。
- 对于一个合理的基础任务粒度来说，构建和管理一个线程的代价甚至可以比任务执行本身所花费的代价更大。

所有这些框架都采用和操作系统把线程映射到`CPU`上相同的方式来把任务映射到线程上。只是他们会使用`fork/join`程序的简单性、常规性以及一致性来执行这种映射。尽管这些框架都能适应不同形式的并行程序，他们优化了`fork/join`的设计：

- 一组工作者线程池是准备好的。在`Java`中，虚拟机和操作系统需要相互结合来完成线程到处理器的映射。然后对于计算密集型的运算来说，这种映射对于操作系统来说是一种相对简单的任务。任何合理的映射策略都会导致线程映射到不同的处理器。
- 所有的`fork/join`任务都是轻量级执行类的实例，而不是线程实例。
- 我们将采用一个特殊的队列和调度原则来管理任务并通过工作线程来执行任务。这些机制是由任务类中提供的相关方式实现的：主要是由`fork`、`join`、`isDone`（一个结束状态的标示符），和一些其他方便的方法，例如调用`coInvoke`来分解合并两个或两个以上的任务。
- 一个简单的控制和管理类（这里指的是`FJTaskRunnerGroup`）来启动工作线程池，并初始化执行一个由正常的线程调用所触发的`fork/join`任务（就类似于`Java`程序中的`main`方法）。

`fork/join`框架的核心在于轻量级调度机制。当工作线程碰到`join`操作时，如果可能的话它将处理其他任务，直到目标任务被告知已经结束（通过`isDone`）。所有的任务都会无阻塞的完成。一个不对称的规则将用于防止潜在的冲突：`pop`会重新检查状态并在获取锁之后继续（对`take`所持有的也一样），直到队列真的为空才退出。在抢断式工作框架中，工作线程对于他们所运行的程序对同步的要求一无所知。他们只是产生（`generate`）、`push`、`pop`、`take`任务、管理任务状态和执行任务。这种简单的方案使得当所有的线程都拥有很多任务需要去执行的时候，它的效率很高。然而这种方式是有代价的，当没有足够的工作的时候它将依赖于试探法。`fork/join`程序在运行时会产生巨大数量的任务单元，然而这些任务在被执行之后又会很快转变为内存垃圾。尽管所展示的可伸缩性结果针对的是单个`JVM`，但根据经验这些主要的发现在更一般的情况下应该仍然成立：

- 尽管分代`GC`（`generational GC`）通常与并行协作得很好，但当垃圾生成速度很快而迫使`GC`很频繁时会阻碍程序的伸缩性。
- 大多数的伸缩性问题只有当运行的程序所用的`CPU`多于多数设备上可用`CPU`时，才会显现出来。
- 应用程序的特征（包括内存局部性、任务局部性和全局同步的使用）常常比框架、`JVM`或是底层`OS`的特征对于伸缩性和绝对性能的影响更大。

### 参考资料

1. [Java Fork/Join 框架](https://github.com/oldratlee/translations/blob/master/a-java-fork-join-framework/README.md)

## 常见类

### String, StringBuffer, StringBuilder

`StringBuffer`和`StringBuilder`都是利用可变数组实现的，二者都继承了`AbstractStringBuilder`，区别仅在于最终方法是否加了`synchronized`，构建时初始的字符串长度为 16，所以一个字符串最大的大小为 2^16 大小，在 JDK9 提供了`StringConcatFactory`作为一个统一的入口。

在字符串拼接的时候，在 JDK 8 中，字符串拼接操作会自动被 javac 转换为 `StringBuilder` 操作，而在 JDK 9 里面则是因为 Java 9 为了更加统一字符串操作优化，提供 了 `StringConcatFactory` ，

由于 String 在 Java 世界中使用过于频繁，Java 为了避免在一个系统中产生大量的 String 对象， 引入了字符串常量池。其运行机制是：创建一个字符串时，首先检查池中是否有值相同的字符串对 象，如果有则不需要创建直接从池中刚查找到的对象引用；如果没有则新建字符串对象，返回对象引用，并且将新创建的对象放入池中。但是，通过 new 方法创建的 String 对象是不检查字符串 池的，而是直接在堆区或栈区创建一个新的对象，也不会把对象放入池中。上述原则只适用于通过直接量给 String 对象引用赋值的情况。

在 Java 9 中，我们引入了 Compact Strings 的设计，对字符串进行了大刀阔斧的改进。将数据存储方式从 char 数组，改变为一个 byte 数组加上一个标识编码的所谓 coder，并且将 相关字符串操作类都进行了修改。另外，所有相关的 Intrinsic 之类也都进行了重写，以保证没有任何性能损失。

由于 String 在 Java 世界中使用过于频繁，Java 为了避免在一个系统中产生大量的 String 对象， 引入了字符串常量池。其运行机制是：创建一个字符串时，首先检查池中是否有值相同的字符串对 象，如果有则不需要创建直接从池中刚查找到的对象引用；如果没有则新建字符串对象，返回对象引用，并且将新创建的对象放入池中。但是，通过 new 方法创建的 String 对象是不检查字符串 池的，而是直接在堆区或栈区创建一个新的对象，也不会把对象放入池中。上述原则只适用于通过直接量给 String 对象引用赋值的情况。

## 代理和反射

静态代理：事先写好代理类，可以手工编写，也可以用工具生成。缺点是每个业务类都要对应一个代理类，非常不灵活。

动态代理：运行时自动生成代理对象。缺点是生成代理代理对象和调用代理方法都要额外花费时间。

JDK 动态代理：基于 Java 反射机制实现， 必须要实现了接口的业务类才能用这种办法生成代理对象。新版本也开始结合 ASM 机制。

cglib 动态代理：基于 ASM 机制实现， 通过生成业务类的子类作为代理类。

反射最大的作用之一就在于我们可以不在编译时知道某个对象的类型，而在运行时通过提供完整的」`包名+类名.class`」得到。注意：不是在编译时，而是在运行时。反射技术常用在各类通用框架开发中。因为为了保证框架的通用性，需要根据配置文件加载不同的对象或类，并调用不同的方法，这个时候就会用到反射——运行时动态加载需要加载的对象。

### 动态代理

### 反射

在 Java 9 以后，这个方法的使用可能会存在一些争议，因为 Jigsaw 项目新增的模块化系统，出于强封装性的考虑，对反射访问进行了限制。Jigsaw 引入了所谓 Open 的概念， 只有当被反射操作的模块和指定的包对反射调用者模块 Open，才能使用 setAccessible；否则，被认为是不合法（ illegal ）操作。如果我们的实体类是定义在模块里面，我们需要在 模块描述符中明确声明：

```
module MyEntities {    // open for reflection    opens com.example to java.persisence}
```

通过代理可以让调用者与实现者之间解耦。

JDK Proxy 的优势：

- 最小化依赖关系，减少依赖意味着简化开发和维护，JDK 本身的支持，可能比 cglib 更加可靠。 极客时间
- 平滑进行 JDK 版本升级，而字节码类库通常需要进行更新以保证在新版 Java 上能够使用。
- 代码实现简单。

基于类似 cglib 框架的优势：

- 有的时候调用目标可能不便实现额外接口，从某种角度看，限定调用者实现接口是有些侵入性的实践，类似 cglib 动态代理就没有这种限制。
- 只操作我们关心的类，而不必为其他相关类增加工作量。
- 高性能。

我们在选型中，性能未必是唯一考量，可靠性、可维护性、编程工作量等往往是更主要的考虑因素，毕竟标准类库和反射编程的门槛要低得多，代码量也是更加可控的，如果我们比 较下不同开源项目在动态代理开发上的投入，也能看到这一点。

代理模式主要涉及三个要素：

1. 抽象类接口
2. 被代理类
3. 动态代理类：实际被调用被代理类的方法和属性的类

## 集合

## List

```
@startuml
interface Collection
interface Queue
interface List
interface Set
interface Deque
interface SortedSet
interface RandomAccess
interface NavigableSet

abstract AbstractCollection
abstract AbstractList
abstract AbstractSequentialList
abstract AbstractQueue
abstract AbstractSet

Collection <|.. AbstractCollection
Collection <|-- List
Collection <|-- Queue
Collection <|--Set

AbstractCollection <|-- AbstractList
AbstractCollection <|-- ArrayDeque
AbstractCollection <|-- AbstractQueue

AbstractList <|-- ArrayList
AbstractList <|-- Vector
AbstractList <|-- AbstractSequentialList

List <|.. AbstractList
List <|.. Vector
List <|.. ArrayList

RandomAccess <|.. Vector
RandomAccess <|.. ArrayList

Vector <|-- Stack

AbstractSequentialList <|-- LinkedList

Deque <|.. ArrayDeque

Queue <|-- Deque
Queue <|.. AbstractQueue

AbstractQueue <|-- PriorityQueue

HashSet <|-- LinkedHashSet

Set <|.. LinkedHashSet
Set <|.. HashSet
Set <|-- SortedSet
Set <|.. AbstractSet

SortedSet <|-- NavigableSet

AbstractSet <|-- HashSet
AbstractSet <|-- TreeSet

NavigableSet <|.. TreeSet
@enduml
```

对于原始数据，目前使用`Dual-Pivot QuickSort`

对于对象数据，目前使用`TimSort`

`ArrayList`内部用数组实现

`LinkedList`内部采用双向列表实现

`Vector`内部采用数组实现

`TreeSet`支持自然顺序访问，但是添加、删除、包含等操作要相对低效（log(n)时间）。`HashSet`则是利用哈希算法，理想情况下，如果哈希散列正常，可以提供常数时间的添加、删除、包含等操作，但是它不保证有序。`LinkedHashSet`，内部构建了一个记录插入顺序的双向链表，因此提供了按照插入顺序遍历的能力，与此同时，也保证了常数时间的添加、删除、包含等操作，这些操作性能略 低于 HashSet，因为需要维护链表的开销。

在遍历元素时，HashSet 性能受自身容量影响，所以初始化时，除非有必要，不然不要将其背后的 HashMap 容量设置过大。而对于 LinkedHashSet，由于其内部链表提供的方 便，遍历性能只和元素多少有关系。

### Map

```
@startuml
interface Map
interface NavigableMap
interface SortedMap

abstract AbstractMap
abstract Dictionary

Map <|.. HashMap
Map <|.. AbstractMap
Map <|.. LinkedHashMap
Map <|.. HashTable
Map <|-- NavigableMap

AbstractMap <|-- HashMap
AbstractMap <|-- EnumMap
AbstractMap <|-- TreeMap

HashMap <|-- LinkedHashMap

Dictionary <|-- HashTable

HashTable <|-- Properties

NavigableMap <|.. TreeMap

SortedMap <|-- NavigableMap

@enduml
```

### LinkedHashMap

`LinkedHashMap`通常提供的是遍历顺序符合插入顺序，它的实现是通过为条目（键值对）维护一个双向链表。通过特定构造函数，我们可以创建反映访问顺序的实例，所谓的 `put` 、 `get` 、 `compute` 等，都算作 「访问」。

其实**顺序性**存储的

### TreeMap

TreeMap 的整体顺序是由键的顺序关系决定的，通过`Comparator`或`Comparable`（自然顺序）来决定

### HashMap

HashMap 的性能表现非常依赖于哈希码的有效性。

### 结构

其可以被看做是数组和链表组成的复合结构，数组被分成一个个桶，通过哈希值决定了键值对在这个数组的寻址；哈希值相同的键值对则以链表形式存储。

HashMap 是按照`lazy-load`原则，在首次使用时被初始化，在声明的时候没有进行初始化操作。

### 负载因子

负载因子选择`0.75`的原因，在理想情况下，使用随机哈希，节点在哈希桶中出现的概率满足泊松分布：

![[assets/c971dbccbde5080f2d6b1553f5c6daf3_MD5.jpg]]

概率定义

这里的 key 并不是对象本身的 hashcode，这是因为：有些数据计算出的哈希值差异主要在高位，而 HashMap 里的哈希寻址是忽略容易以上的高位的，这种处理就可以有效避免类似情况下的哈希碰撞。这也是源码中`hash`方法要右移 16 位的原因。

### 为什么 HashMap 要树化呢

本质上这是个安全问题。 因为在元素放置过程中，如果一个对象哈希冲突，都被放置到同一个桶里，则会形成一个链表，我们知道链表查询是线性的，会严重影响存取的性能。 而在现实世界，构造哈希冲突的数据并不是非常复杂的事情，恶意代码就可以利用这些数据大量与服务器端交互，导致服务器端 CPU 大量占用，这就构成了哈希碰撞拒绝服务攻击。

### 解决哈希冲突的方法

1. 开放地址法
2. 再哈希法
3. 链地址法

需要使用多线程下安全的`Map`可以使用：`Collections`的`synchronizedMap`或者`CurrentHashMap`

### ConcurrentHashMap

### 实现

早期的实现：

- 分离锁，也就是将内部进行分段（Segment），里面则是 HashEntry 的数组，和 HashMap 类似，哈希相同的条目也是以链表形式存放
- HashEntry 内部使用 volatile 的 value 字段来保证可见性，也利用了不可变对象的机制以改进利用 Unsafe 提供的底层能力，比如 volatile access，去直接完成部分操作，以最优化性能，毕竟 Unsafe 中的很多操作都是 JVM intrinsic 优化过的。

JDK7 的实现：

- `get`操作要保证可见性
- 对于`put`操作，首先是通过二次哈希避免哈希冲突，然后以`Unsafe`调用方式，直接获取对应的`Segment`，然后线程安全的进行`put`操作
- ConcurrentHashMap 会获取再入锁，以保证数据一致性，Segment 本身就是基于 ReentrantLock 的扩展实现，所以，在并发修改期间，相应 Segment 是被锁定的。
- 在最初阶段，进行重复性的扫描，以确定相应 key 值是否已经在数组里面，进而决定是更新还是放置操作。
- 当对其进行扩容的时候，它进行的不再是整体的扩容，而是对单独的`Segment`进行扩容
- 如果不进行同步，简单的计算所有`Segment`值，可能会因为并发 put，导致结果不准确，但是直接锁定所有`Segment`进行计算，就会边得非常昂贵。分离锁也限制了 Map 的初始化操作。ConcurrentHashMap 的实现是通过重试机制（ RETRIES_BEFORE_LOCK，指定重试次数 2 ），来试图获得可靠值。如果没有监控到发生变化（通过对 比 Segment.modCount ），就直接返回，否则获取锁进行操作。

JDK8 的实现：

- 总体结构上，它的内部存储变得和 HashMap 结构非常相似，同样是大的桶（bucket）数组，然后内部也是一个个所谓的链表结构（bin），同步的粒度要 更细致一些。
- 其内部仍然有 Segment 定义，但仅仅是为了保证序列化时的兼容性而已，不再有任何结构上的用处。
- 因为不再使用 Segment，初始化操作大大简化，修改为 lazy-load 形式，这样可以有效避免初始开销
- 数据存储利用 volatile 来保证可见性。
- 使用 CAS 等操作，在特定场景进行无锁并发操作。
- 使用 Unsafe、LongAdder 之类底层手段，进行极端情况的优化。 1.8 以后的锁的颗粒度，是加在链表头上的，这个是个思路上的突破:
- 自旋锁是 cas 的一种应用方式。并发包中的原子类是典型的应用。
- 偏向锁个是获取锁的优化。在 ReentrantLock 中用于实现已获取完锁的的线程重入问题。

## IO

```
@startuml

interface Closeable

Object <|- File
Object <|- RandomAccessFile
Object <|- InputStream
Object <|- OutputStream
Object <|- Reader
Object <|- Writer

Closeable <|. File
Closeable <|. RandomAccessFile
Closeable <|. InputStream
Closeable <|. OutputStream
Closeable <|. Reader
Closeable <|. Writer

@enduml
```

InputSteam/OutputStream 是用于读取或写入字节的，而 Reader/Writer 则是用于操作字符的，增加了字符编码等功能，适用于类似从文件中读取或者写入文本信息，这种设计利用了缓冲区，将批量数据进行一次操作

### NIO

- Buffer：高效的数据容器，除了布尔类型，所有的原始类型都有相应的 Buffer 实现
- Channel，类似在 Linux 之类操作系统上看到的文件描述符，是 NIO 中被用来支持批量式 IO 操作的一种抽象。
- File 或者 Socket，通常认为是比较高层次的抽象
- Selector，是 NIO 实现多路复用的基础，它提供了一种高效的机制，可以检测到注册在 Selector 上的多个 Channel 中，是否有 Channel 处于就绪状态，进而实现了单线程对 多 Channel 的高效管理。Selector 是基于底层操作系统机制，在 Linux 上依赖于 epoll，在 Windows 上则依赖于 iocp
- Chartset，提供 Unicode 字符串定义，NIO 也提供了相应的编解码器等，例如，通过下面的方式进行字符串到 ByteBufer 的转换

IO 都是同步阻塞模式，所以需要多线程以实现多任务处理。而 NIO 则是利用了单线程轮询事件的机制，通过高效地定位就绪的 Channel，来决定做什 么，仅仅 select 阶段是阻塞的，可以有效避免大量客户端连接时，频繁线程切换带来的问题，应用的扩展能力有了非常大的提高。

JDK 的源代码中，内部实现和公共 API 定义也不是刻意能够简单的关联上的，NIO 部分代码甚至是定义为模板而不是 Java 源文件，在 build 过程中自动生成源码。

基于 NIO transferTo 实现方式，在 Linux 和 Unix 上，则会使用到零拷贝技术，数据传输并不需要用户参与，省去了上下文切换的开销和不必要的内存拷贝，进而可能提高应用拷贝性能。

### NIO Buffer

- capcity，它反映这个 Bufer 到底有多大，也就是数组的长度。
- position，要操作的数据起始位置。
- limit，相当于操作的限额。在读取或者写入时，limit 的意义很明显是不一样的。比如，读取操作时，很可能将 limit 设置到所容纳数据的上限；而在写入时，则会设置容量或容量 以下的可写限度。
- mark，记录上一次 postion 的位置，默认是 0，算是一个便利性的考虑，往往不是必须的。

`MappedByteBuffer`它将文件按照指定大小直接映射为内存区域，当程序访问这个内存区域时，将直接操作这块文件数据，省去了将数据从内核空间想用户空间传输的损耗。

在实际使用过程中，Java 会尽量对`Direct Buffe`仅做本地 IO 操作，对于很多大数据两的 IO 密集操作，可能会带来非常大的性能优势：

- `Direct Buffer`在生命周期内不会发生改变，进而内核可以安全地对其进行访问，很多 IO 操作会很高效
- 减少堆内对象存储的可能额外维护工作，所以访问效率可能有所提高

但是创建和销毁的过程中，都会比一般的内存 Buffer 增加部分开销，所有通常用于长期使用、数据量较大的场景。

大多数垃圾收集过程中，都不会主动收集 Direct Bufer，它的垃圾收集过程，就是基于我在专栏前面所介绍的 Cleaner （一个内部实现）和幻象引用 （ PhantomReference ）机制，其本身不是 public 类型，内部实现了一个 Deallocator 负责销毁的逻辑。对它的销毁往往要拖到 full GC 的时候，所以使用不当很容易导 致 OutOfMemoryError。

Direct Bufer 的回收：

- 在应用程序中，显示地调用`System.gc()`来强制回收
- 在大量使用 Direct Bufer 的部分框架中，框架会自己在程序中调用释放方法
- 重复使用

## 锁

```
@startuml
interface Lock

Lock <|. ReentrantLook
Lock <|. ReentrantReadWriteLock.ReadLock
Lock <|. ReentrantReadWriteLock.WriteLock

@enduml
```

### 基础

保证线程安全的两个方法：

- 封装
- 不可变

线程安全需要保证的几个基本特性：

- 原子性
- 可见性
- 有序性

当公平性为真的时候，会倾向将锁赋予等待时间最久的线程，公平性是为了解少线程「饥饿」的一个方法。建议只有当程序确实需要有公平性需求的时候，才有必要指定它。

### synchronized

`synchronizied`是 Java 提供的内建同步机制，提供了互斥语义和可见性，当一个线程已经获取当前锁时，其他试图获取的线程只能等待或者阻塞在那里synchronized 代码块是由一对儿 monitorenter/monitorexit 指令实现的，Monitor 对象是同步的基本实现单元 。 在 Java 6 之前，Monitor 的实现完全是依靠操作系统内部的互斥锁，因为需要进行用户态到内核态的切换，所以同步操作是一个无差别的重量级操作。现在的 JDK 提供了三种不同的`Monitor`实现：偏向锁、轻量级锁、重量级锁

锁降级：锁降级确实会发生，当 JVM 进入 SafePoint 的时候，会检查是否有闲置的 Monitor，然后试图进行降级。

当没有竞争出现时，默认会使用偏向锁。JVM 会利用 CAS 操作在对象头上的 Mark Word 部分设置线程 ID，以表示这个对象偏向于当前线程，所以并不会涉及真正的互斥锁。

### ReentrantLock

可再入：表示当一个线程试图获取一个它已经获取的锁的时候，这个获取动作就自动成功，锁的持有是以线程为单位而不是基于调用次数。Java 锁实现强调再入性是为了和 pthread 的行为进行区分。

在使用 ReentrantLock 类的时，一定要注意三点：

- 在 fnally 中释放锁，目的是保证在获取锁之后，最终能够被释放。
- 不要将获取锁的过程写在 try 块内，因为如果在获取锁时发生了异常，异常抛出的同时，也会导致锁无故被释放。
- ReentrantLock 提供了一个 newCondition 的方法， 以便用户在同一锁的情况下可以根据不同的情况执行等待或唤醒的动作。

### Condition

`Condition`则是将 wait、notify、notifyall 等操作转化为相应的对象，将复杂而晦涩的同步操作转变为直观可控的对象行为

### 自旋锁

竞争锁的失败的线程，并不会真实的在操作系统层面挂起等待，而是 JVM 会让线程做几个空循环(基于预测在不久的将来就能获得)，在经过若干次循环后，如果可以获得锁，那么进入临界区，如果还不能获得锁，才会真实的将线程在操作系统层面进行挂起。适用场景:自旋锁可以减少线程的阻塞，这对于锁竞争不激烈，且占用锁时间非常短的代码块来说，有较大的性能提升，因为自旋的消耗会小于线程阻塞挂起操作的消耗。 如果锁的竞争激烈，或者持有锁的线程需要长时间占用锁执行同步块，就不适合使用自旋锁了，因为自旋锁在获取锁前一直都是占用 cpu 做无用功，线程自旋的消耗大于线程阻塞挂起操作的消 耗，造成 cpu 的浪费。

CountDownLatch 和 CyclicBarrier 的区别：

- CountDownLatch 是不可以重置的，所以无法重用；而 CyclicBarrier 则没有这种局限性，可以重用
- CountDownLatch 的基本操作组合是 countDown / await
- CyclicBarrier 的基本操作组合是 awit

## 线程

### 状态

![[assets/9a2615475810f0e70f0f185b0b46074c_MD5.svg]]

- 新建（NEW）
- 就绪（RUNABLE）
- 阻塞（BLOCKED）
- 等待（WAITING）
- 计时等待（TIMED_WAIT）

`Rannable`的好处是不会受 Java 不支持类多继承的限制，重用代码实现。

影响线程状态的因素：

- 线程的自身方法
- 基类 Object 提供了一些基础的 wait / notify/ notifyall 方法
- 并发类库中的工具

慎用`ThreadLocal`，其在整个线程生命周期内有效，所以可以方便地在一个线程关联的不同业务模块之间传递信息。

### 死锁

产生死锁的的原因：

- 互斥条件
- 互斥条件是长期持有的，在使用结束之前，自己不会释放，也不能被其他线程抢占
- 循环依赖关系，两个或者多个个体之间出现了锁的链条环

避免死锁的思路：

- 尽可能的避免使用多个锁，并且只有需要的时候才持有锁
- 如果必须要使用多个锁，尽量设计好锁的获取顺序
- 使用带超时的方法，为程序带来更多可控性

### 线程池

`Executor`是一个基础接口，其初衷是将任务提交和任务执行细节解耦。

`ThreadPoolExecutor`的设计：

- 工作队列负责存储用户提交的各个任务，这个工作队列，可以是容量为 0 的`SynchronousQueue`，也可以是固定大小的的线程池。
- 内部的「线程池」，是指保持工作线程的集合，线程池需要在运行的过程中管理线程创建、销毁
- 线程池的工作下城被抽象为静态内部类`Worker`，基于 AOS 实现
- ThreadFactory 提供创建线程所需要的逻辑
- 如果任务提交时被拒绝，需要为其提供处理逻辑

使用线程池时候需要注意的问题：

- 避免任务堆积
- 避免过度扩展线程
- 如果线程数目不断增长，要警惕线程泄漏
- 避免死锁等同步问题
- 尽量不免在使用线程池时操作 `ThreadLocal`，因为工作线程的生命周期通常都会超过任务的生命周期

### CAS

CAS 也并不是没有副作用，其常用的失败重试机制中，隐含着一个驾驶，即竞争情况是短暂的。

对于 ABA 问题，Java 提供了`AtomicStampedRefrence`工具类，通过建 stamp 的方式来保证 CAS 的正确性

### AQS

### 设计初衷

从原理上，一种同步结构往往是可以利用其它的结构实现的；但是对于某种同步结构的倾向，会导致复杂、晦涩的实现逻辑，所以选择了将基础的同步相关操作抽象在`AbstractQueuedSynchrogazer`中，利用 AQS 提供构建同步结构的范本

### 基本结构

- 一个 volatile 的整数成员表征状态，同事提供 getState 和 setState 方法
- 一个先入先出的等待线程队列，以实现多线程间竞争和等待，这是 AQS 机制的核心之一
- 各种基于 CAS 的基操作，以及各种期望具体同步结构去实现的 acquire / release 方法

## 类加载

Java 类加载过程分为三个主要步骤：加载、链接、初始化

通常类加载机制有三个基本特征：

- 双亲委派模型
- 可见性，子类加载器可以访问父类加载器加载的类型，但是反过来不允许
- 单一性，由于父加载器的类型对于子加载器是可见的，所以父加载器中加载过的类型，就不会再子加载器中重复加载

## 类加载

### 动态代理

对于一个普通的动态代理，其实现过程可以简化为：

- 提供一个基础的接口，作为被调用类型（com.mycorp.HelloImpl）和代理类之间的统一入口
- 实现[InvocationHandler](https://docs.oracle.com/javase/9/docs/api/java/lang/reflect/InvocationHandler.html)，对代理对象方法的调用，会被分派到其 invoke 方法来真正实现动作
- 通过 Proxy 类，调用其 newProxyInstance 方法，生成一个实现了相应基础接口的代理类实例，可以看下面的方法签名。

## 内存

### JVM 内存区域

![[assets/27e2ee940068fd363fec8932cb902c1f_MD5.jpg]]

java内存区域

1. 程序计数器
2. Java 虚拟机栈
3. 堆
4. 方法区
5. 运行时常量池
6. 本地方法栈

### OOM

### OutOfMemoryEroor 的原因

- 堆内存不足是最常见的 OOM 原因之一，抛出的信息是`java.lang.OutOfMemeryError:Java heap space`
- 对于 Java 虚拟机栈和本地方法栈，当递归调用的时候没给退出条件，就会导致不断的压栈，这种情况，一般会抛出`StactOverFlowError`，但是当 JVM 试图去扩展栈空间的时候失败就会抛出`OutOfMemoryError`
- 老版本的 JDK 中永久代的大小是有限的，并且对永久代的垃圾回收不积极，所以当不断添加新类型时候，永久代会出现`OutOfMemoryError`，尤其在运行时存在大量动态类型生成的场合
- 随着元数据的引入，方法区的 OOM 有所改观，现在异常信息变成`java.lang.OutOfMemory:Metaspace`
- 直接内存不足

## 垃圾回收

### 常见的垃圾回收器

- Serial GC：其收集工作是单线程的，并且在进行垃圾回收过程中，会进入「Stop-The-World」的状态
- ParNew GC：是`Serial GC`的多线程版
- CMS GC：基于标记——清除算法，设计目的是尽量减少停顿时间，其存在内存碎片化问题，所以难以避免在长时间运行等情况下发生`full GC`，另外，因为并发的原因，CMS 会占用更多 CPU 资源，并和用户线程争抢
- Parrallel GC：其特点是新生代和老年代 GC 都是并行进行的，在常见的服务器黄金中更加高效
- G1 GC：兼顾吞吐量和停顿时间的 GC 实现。G1 仍然存在着年代的概念，但是其内存结构并不是简单的条带式划分，而是类似棋盘的一个个 region。Region 之间是复制算法，但整体上时间可以看做是标记——整理算法，可以有效避免内存碎片

### 对象实例收集

对于对象实例收集，主要是两种基本算法，引用计数和可达性分析。

- 引用计数，就是为对象添加一个引用计数，用于记录对象被引用的情况，如果计数为 0，即表示回收
- 可达性分析，就是讲对象及其引用关系看做一个图，选定活动的对象作为 GC Roots，然后跟踪引用链条，如果一个对象和 GC Roots 之间不可达，也就是不存在引用链条，那么即可以认为是可回收对象。JVM 会把虚拟机栈和本地方法栈中正在引用的对象、静态属性引用的对象和常量，作为 GC Roots

### 算法

- 复制
- 标记——清除
- 标记——整理

G1 的简化理解：

- 在新生代，G1 采用的仍然是并行的复制算法，所以同样会发生 Stop-The-World 的暂停
- 在老年代，大部分情况下都是并发标记，而整理则是和新生代 GC 是捎带进行，并且不是整体性的整理，而是增量进行的

### 调优

- 理解应用需求和问题，确定调优目标
- 掌握 JVM 和 GC 的状态，定位具体问题，确定真的有 GC 调优的必要
- 选择的 GC 类型是否符合我们的引用特征
- 通过分析确定具体调整的参数或者软硬件配置
- 验证是否达到调优目标

### JMM

需要 JMM 的原因是因为过于泛泛的内存模型定义：

- 既不能保证一些多线程程序的正确性
- 也不能保证同一段程序在不同处理器结构上的一致

### Docker

用老版本 JDK（JDK 8u131)之前的应该注意：

- 明确设置堆、元数据区等内存区域大小，保证 Java 进程的总大小可控
- 明确配置 GC 和 JIT 并行线程的数目，以避免二者占用过多计算资源

在 JDK9 以后可以使用 Jlink 工具定制最小依赖的 Java 运行环境

## 安全

### 组成

1. 运行时安全机制
2. 在类加载过程中，进行字节码验证，以防止不合规的代码影响 JVM 运行或者载入其他恶意代码
3. 类加载器本身也可以对代码之间进行隔离
4. 利用`SecurityManager`机制和相关的组件，限制代码的运行时行为能力，其中，可以定制`policy`文件和各种粒度的权限定义，限制代码的作用域和权限
5. 从原则上来说，Java 的 GC 等资源回收管理机制，都可以看作是运行时安全的一部分，如果相应机制失效，就会导致 JVM 出现 OOM 等错误，可看作是另类的拒绝服务
6. Java 提供的安全框架 API，这是构建安全通信等应用的基础
7. JDK 集成的各种安全工具

敏感的信息不要被序列化，在编码中，建议使用 transient 关键字将其保护起来
