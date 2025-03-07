## undo log

是 Innodb 存储引擎层生成的日志，实现了事务中的**原子性**，主要**用于事务回滚和 MVCC**。

undo log 是一种用于撤销回退的日志。在事务没提交之前，MySQL 会先记录更新前的数据到 undo log 日志文件里面，当事务回滚时，可以利用 undo log 来进行回滚。

在发生回滚时，就读取 undo log 里的数据，然后做原先相反操作。

**undo log 还有一个作用，通过 ReadView + undo log 实现 MVCC（多版本并发控制）**

对于「读提交」和「可重复读」隔离级别的事务来说，它们的快照读（普通 select 语句）是通过 Read View + undo log 来实现的，它们的区别在于创建 Read View 的时机不同：

- 「读提交」隔离级别是在每个 select 都会生成一个新的 Read View，也意味着，事务期间的多次读取同一条数据，前后两次读的数据可能会出现不一致，因为可能这期间另外一个事务修改了该记录，并提交了事务。
- 「可重复读」隔离级别是启动事务时生成一个 Read View，然后整个事务期间都在用这个 Read View，这样就保证了在事务期间读到的数据都是事务启动前的记录。

undo log 两大作用：

- **实现事务回滚，保障事务的原子性**。事务处理过程中，如果出现了错误或者用户执 行了 ROLLBACK 语句，MySQL 可以利用 undo log 中的历史数据将数据恢复到事务开始之前的状态。
- **实现 MVCC（多版本并发控制）关键因素之一**。MVCC 是通过 ReadView + undo log 实现的。undo log 为每条记录保存多份历史数据，MySQL 在执行快照读（普通 select 语句）的时候，会根据事务的 Read View 里的信息，顺着 undo log 的版本链找到满足其可见性的记录。
## redo log

是 Innodb 存储引擎层生成的日志，实现了事务中的**持久性**，主要**用于掉电等故障恢复**

redo log 是物理日志，记录了某个数据页做了什么修改，比如**对 XXX 表空间中的 YYY 数据页 ZZZ 偏移量的地方做了 AAA 更新**，每当执行一个事务就会产生这样的一条或者多条物理日志。

在事务提交时，只要先将 redo log 持久化到磁盘即可，可以不需要等到将缓存在 Buffer Pool 里的脏页数据持久化到磁盘。

所以有了 redo log，再通过 WAL 技术，InnoDB 就可以保证即使数据库发生异常重启，之前已提交的记录都不会丢失，这个能力称为 **crash-safe**（崩溃恢复）。可以看出来， **redo log 保证了事务四大特性中的持久性**。

当系统崩溃时，虽然脏页数据没有持久化，但是 redo log 已经持久化，接着 MySQL 重启后，可以根据 redo log 的内容，将所有数据恢复到最新的状态

## redo log 和undo log的区别

- redo log 记录了此次事务「**修改后**」的数据状态，记录的是更新**之后**的值，**主要用于事务崩溃恢复，保证事务的持久性**。
- undo log 记录了此次事务「**修改前**」的数据状态，记录的是更新**之前**的值，**主要用于事务回滚，保证事务的原子性**。

## binlog

MySQL 在完成一条更新操作后，Server 层还会生成一条 binlog，等之后事务提交的时候，会将该事物执行过程中产生的所有 binlog 统一写 入 binlog 文件。主要是用于主从复制、备份恢复使用

如果不小心删除了整个库，只能使用binlog进行恢复

MySQL 集群的主从复制过程梳理成 3 个阶段：

- **写入 Binlog**：主库写 binlog 日志，提交事务，并更新本地存储数据。
- **同步 Binlog**：把 binlog 复制到所有从库上，每个从库把 binlog 写到暂存日志中。
- **回放 Binlog**：回放 binlog，并更新存储引擎中的数据。

## 一条查询语句的执行流程

查询语句的那一套流程，更新语句也是同样会走一遍：

- 客户端先通过连接器建立连接，连接器自会判断用户身份；
- 因为这是一条 update 语句，所以不需要经过查询缓存，但是表上有更新语句，是会把整个表的查询缓存清空的，所以说查询缓存很鸡肋，在 MySQL 8.0 就被移除这个功能了；
- 解析器会通过词法分析识别出关键字 update，表名等等，构建出语法树，接着还会做语法分析，判断输入的语句是否符合 MySQL 语法；
- 预处理器会判断表和字段是否存在；
- 优化器确定执行计划，因为 where 条件中的 id 是主键索引，所以决定要使用 id 这个索引；
- 执行器负责具体执行，找到这一行，然后更新。

ref:
- [MySQL 日志：undo log、redo log、binlog 有什么用？ | 小林coding](https://xiaolincoding.com/mysql/log/how_update.html#%E6%80%BB%E7%BB%93)

## MySQL中的组件

分层主要分为两层Server层和存储引擎层

主要组件可以根据一条查询语句的执行过程就能说出来：

1. 连接器
2. 缓存
3. 分析器
	1. 词法分析
	2. 语法分析
4. 优化器
5. 执行器

ref: 
- [MySQL - 一文了解MySQL的基础架构及各个组件的作用_mysql框架干嘛用的-CSDN博客](https://blog.csdn.net/weixin_42201180/article/details/125683385)

##  MyISAM 和 InnoD

B 树：每个节点都存储 key 和 data，所有节点组成这棵树，并且叶子节点指针为 null。

B + 树： 只有叶子节点存储 data，叶子节点包含了这棵树的所有键值，叶子节点不存储指针。后来，在 B + 树上增加了顺序访问指针，也就是每个叶子节点增加一个指向相邻叶子节点的指针，这样一棵树成了数据库系统实现索引的首选数据结构

为什么使用b+数作为首选的数据结构：一般来说，索引很大，往往以索引文件的形式存储的磁盘上，索引查找时产生磁盘 I/O 消耗，相对于内存存取，I/O 存取的消耗要高几个数量级，所以评价一个数据结构作为索引的优劣最重要的指标就是在查找过程中磁盘 I/O 操作次数的时间复杂度。树高度越小，I/O 次数越少。

MyISAM ：data 存的是数据地址。索引是索引，数据是数据。
InnoDB：data 存的是数据本身。索引也是数据。

两种存储引擎的区别：

1. MyISAM 是非事务安全的，而 InnoDB 是事务安全的
2. MyISAM 锁的粒度是表级的，而 InnoDB 支持行级锁
3. MyISAM 支持全文类型索引，而 InnoDB 不支持全文索引
4. MyISAM 相对简单，效率上要优于 InnoDB，小型应用可以考虑使用 MyISAM
5. MyISAM 表保存成文件形式，跨平台使用更加方便
6. MyISAM 管理非事务表，提供高速存储和检索以及全文搜索能力，如果在应用中执行大量 select 操作可选择
7. InnoDB 用于事务处理，具有 ACID 事务支持等特性，如果在应用中执行大量 insert 和 update 操作，可选择。
ref:
- [B树和B+树，InnoDB和MYISAM的区别_mysql中innerdb的b+树和其他执行引擎的b+树有什么区别-CSDN博客](https://blog.csdn.net/wsll581/article/details/83793301)

## 主从复制

MySQL 集群的主从复制过程梳理成 3 个阶段：

- **写入 Binlog**：主库写 binlog 日志，提交事务，并更新本地存储数据。
- **同步 Binlog**：把 binlog 复制到所有从库上，每个从库把 binlog 写到暂存日志中。
- **回放 Binlog**：回放 binlog，并更新存储引擎中的数据。

三种模式：

- **同步复制**：MySQL 主库提交事务的线程要等待所有从库的复制成功响应，才返回客户端结果。这种方式在实际项目中，基本上没法用，原因有两个：一是性能很差，因为要复制到所有节点才返回响应；二是可用性也很差，主库和所有从库任何一个数据库出问题，都会影响业务。
- **异步复制**（默认模型）：MySQL 主库提交事务的线程并不会等待 binlog 同步到各从库，就返回客户端结果。这种模式一旦主库宕机，数据就会发生丢失。
- **半同步复制**：MySQL 5.7 版本之后增加的一种复制方式，介于两者之间，事务线程不用等待所有的从库复制成功响应，只要一部分复制成功响应回来就行，比如一主二从的集群，只要数据成功复制到任意一个从库上，主库的事务线程就可以返回给客户端。这种**半同步复制的方式，兼顾了异步复制和同步复制的优点，即使出现主库宕机，至少还有一个从库有最新的数据，不存在数据丢失的风险**。

## mysql explain

`EXPLAIN` 是 MySQL 中用于分析 SQL 查询执行计划的工具。通过使用 `EXPLAIN`，你可以查看 MySQL 如何执行一条 SQL 查询，包括它如何访问表、使用哪些索引、是否进行了排序或分组等。这对于优化查询性能非常有帮助。

主要关注的指标：

1. type：使用的连接类型
2. key：使用的索引
3. rows：就是出现的扫描行数
4. extra：尽量避免出现 `Using temporary` 和 `Using filesort`，这些通常意味着查询需要优化。

## ACID

- 原子性（Atomicity）：即不可分割性，事务中的操作要么全不做，要么全做
- 一致性（Consistency）：一个事务在执行前后，数据库都必须处于正确的状态，满足[完整性约束](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%AE%8C%E6%95%B4%E6%80%A7%E7%BA%A6%E6%9D%9F)
- 隔离性（Isolation）：多个事务并发执行时，一个事务的执行不应影响其他事务的执行
- 持久性（Durability）：事务处理完成后，对数据的修改就是永久的，即便系统故障也不会丢失

## 数据库的事务隔离级别

![[assets/feafe55b271a7e600339e5c448eb325b_MD5.jpeg]]


脏读： 读取到了未提交的数据
不可重复读：前后多次读取数据不一致。事务A中先后多次读取同一个数据，读取的结果不一样，因为另外一个事务也访问该同一数据，并且可能修改这个数据
幻读：在事务A中按照某个条件先后两次查询数据库，两次查询结果的条数不同，这种现象称为幻读

脏读与不可重复读的区别在于：前者读到的是其他事务未提交的数据，后者读到的是其他事务已提交的数据。

不可重复读与幻读的区别可以通俗的理解为：前者是数据变了，后者是数据的行数变了。


## MySQL大数据量查询不会爆内存的原因

主要是 MySQL 本身有边读边发的机制和 innodb的 buffer pool 管理机制。

1. 边度边发
	1. 逐步读取数据：MySQL 执行查询的时候，是每次读取一部分数据，然后发回给客户端
	2. 客户端接收速度的影响：客户端接收慢，可能会阻塞查询操作，但是不会导致内存不足，最后直到连接超时释放连接
2. innodb 的 buffer pool 管理