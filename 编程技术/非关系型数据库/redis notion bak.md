## 锁设计

建立和释放锁，并保证绝对的安全，是这个锁的设计比较棘手的地方。有两个潜在的陷阱：

1. 应用程序通过网络和`redis`交互，这意味着从应用程序发出命令到`redis`结果返回之间会有延迟
2. 如果程序在获取锁后突然`crash`，而无法释放，这个锁会一直存在导致程序进入「死锁」

使用`SETNX`消除`GET`命令需要等待返回值的问题，`SETNX`只有在可以不存在时才返回成功，但是要释放锁需要显示的删除，这个时候我们可以使用`PEXPIRE`给锁加一个存活时间，一旦超过 TTL，这个锁就会被 Redis 自动删除。这会产生另外一个问题。PEXPIRE 命令没有判断 SETNX 命令的返回结果，无论如何都会设置 key 的 TTL。如果这个地方无法获取到锁或有异常，那么多个线程每次想获取锁时，都会频繁更新 key 的 TTL，这样会一直延长 key 的 TTL，导致 key 永远都不会过期。如果不采用脚本的方式来实现，可以使用 Redis 2.6.12 之后版本 SET 命令的 PX 和 NX 参数来实现。为了考虑兼容 2.6.0 之前的版本，我们还是采用脚本的方式来实现。

> 为什么使用 Redis 脚本来实现分布式锁的原因之一

## Redis 和 Memcached 的区别

1. Redis 支持复杂的数据结构
2. Redis 支持原生的集群模式
3. 由于 redis 只使用**单核**，而 memcached 可以使用**多核**，所以平均每一个核上 redis 在存储小数据时比 memcached 性能更高。而在 100k 以上的数据中，memcached 性能要高于 redis。

## 线程模型

redis 内部使用文件事件处理器 `file event handler`，这个文件事件处理器是单线程的，所以 redis 才叫做单线程的模型。它采用 IO 多路复用机制同时监听多个 socket，将产生事件的 socket 压入内存队列中，事件分派器根据 socket 上的事件类型来选择对应的事件处理器进行处理。文件事件处理器的结构包含 4 个部分：

- 多个 socket
- IO 多路复用程序
- 文件事件分派器
- 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）

多个 socket 可能会并发产生不同的操作，每个操作对应不同的文件事件，但是 IO 多路复用程序会监听多个 socket，会将产生事件的 socket 放入队列中排队，事件分派器每次从队列中取出一个 socket，根据 socket 的事件类型交给对应的事件处理器进行处理。

## Redis 单线程模型也能高效的原因

- 纯内存操作
- 核心是基于非阻塞的 IO 多路复用机制
- C 语言实现……
- 单线程避免了多线程频繁的上下文切换，预防了多线程可能产生的竞争问题

## 数据类型

### string

可以做简单的 KV 缓存

### hash

类似 map 的一种结构，这个一般就是可以将结构化的数据，比如一个对象（没有嵌套其他的对象）给缓存到 Redis 里，然后每次读写缓存的时候，可以操作 hash 里的某个字段

### list

list 是有序列表。可以通过 list 存储一些列表型的数据结构，比如粉丝的列表，文章的评论之类的东西。比如可以通过`lrange`命令，读取某个闭区间内的元素，可以基于 list 实现分页查询，也可以基于 list 实现简单的的消息队列

### set

set 是无序集合，自动去重。可以在多服务之间做全局的去重

### sorted set

排序的 set，去重但是可以排序，写进去的时候给一个分页树，自动根据分页排序

## 过期策略

默认的过期策略是：**定期删除 + 惰性删除**定期删除：Redis 默认是每隔 100ms 就随机抽取一些设置了过期时间的 key，检查其是否过期，如果过期就删除惰性删除：在获取某个 key 的时候，redis 会检查这个 key 是否过期，如果过期了就会删除，不给返回任何东西

如果大量的 ky 堆积，导致 redis 内存块耗尽了，这个时候走的是**内存淘汰机制****

## **内存淘汰机制**

发生在内存不足以容纳新写入数据的时候

- neoviction：新写入操作会报错
- **allkey-lru**：在键空间中，移除最近最少使用的 key
- allkey-random：在键空间中，随机移除某个 key
- volicate-lru：在设置了过期时间的键空间中，移除最近最少使用的 key
- volicate-random：在设置了过期时间的键空间中，随机移除某个 key
- volicate-ttl：在设置了过期时间的键空间中，有更早过期时间的 key 优先移除

## 高可用架构的选择

如果你的数据量很少，主要是承载高并发高性能的场景，比如你的缓存一般就几个 G，单机就足够了，可以使用 replication，一个 master 多个 slaves，要几个 slave 跟你要求的读吞吐量有关，然后自己搭建一个 sentinel 集群去保证 redis 主从架构的高可用性。redis cluster，主要是针对**海量数据+高并发+高可用**的场景。redis cluster 支撑 N 个 redis master node，每个 master node 都可以挂载多个 slave node。这样整个 redis 就可以横向扩容了。如果你要支撑更大数据量的缓存，那就横向扩容更多的 master 节点，每个 master 节点就能存放更多的数据了。

## 主从架构

![[assets/242362f42246d3841071fb93039db1aa_MD5.jpg]]

### 核心机制

- redis 采用**异步方式**复制数据到 slave 节点，不过 redis2.8 开始，slave node 会周期性地确认自己每次复制的数据量；
- 一个 master node 是可以配置多个 slave node 的；
- slave node 也可以连接其他的 slave node；
- slave node 做复制的时候，不会 block master node 的正常工作；
- slave node 在做复制的时候，也不会 block 对自己的查询操作，它会用旧的数据集来提供服务；但是复制完成的时候，需要删除旧数据集，加载新数据集，这个时候就会暂停对外服务了；
- slave node 主要用来进行横向扩容，做读写分离，扩容的 slave node 可以提高读的吞吐量。

### 核心原理

当启动一个 slave node 的时候，它会发送一个 `PSYNC` 命令给 master node。

如果这是 slave node 初次连接到 master node，那么会触发一次 `full resynchronization` 全量复制。此时 master 会启动一个后台线程，开始生成一份 `RDB` 快照文件，同时还会将从客户端 client 新收到的所有写命令缓存在内存中。`RDB` 文件生成完毕后，master 会将这个 `RDB` 发送给 slave，slave 会先**写入本地磁盘，然后再从本地磁盘加载到内存**中，接着 master 会将内存中缓存的写命令发送到 slave，slave 也会同步这些数据。slave node 如果跟 master node 有网络故障，断开了连接，会自动重连，连接之后 master node 仅会复制给 slave 部分缺少的数据。

![[assets/d307fc37137d63ff797905f6125ec6f7_MD5.jpg]]

image.png

## 主从复制的断点续传

master node 会在内存中维护一个 backlog，master 和 slave 都会保存一个 replica offset 还有一个 master run id，offset 就是保存在 backlog 中的。如果 master 和 slave 网络连接断掉了，slave 会让 master 从上次 replica offset 开始继续复制，如果没有找到对应的 offset，那么就会执行一次 `resynchronization`。

> 如果根据 host+ip 定位 master node，是不靠谱的，如果 master node 重启或者数据出现了变化，那么 slave node 应该根据不同的 run id 区分。

### 过期 key 处理

slave 不会过期 key，只会等待 master 过期 key。如果 master 过期了一个 key，或者通过 LRU 淘汰了一个 key，那么会模拟一条 del 命令发送给 slave。

### 复制

slave node 启动时，会在自己本地保存 master node 的信息，包括 master node 的`host`和`ip`，但是复制流程没开始。slave node 内部有个定时任务，每秒检查是否有新的 master node 要连接和复制，如果发现，就跟 master node 建立 socket 网络连接。然后 slave node 发送 `ping` 命令给 master node。如果 master 设置了 requirepass，那么 slave node 必须发送 masterauth 的口令过去进行认证。master node **第一次执行全量复制**，将所有数据发给 slave node。而在后续，master node 持续将写命令，异步复制给 slave node。

![[assets/4f92a7e4abf2595bf366af7bb509d219_MD5.jpg]]

image.png

## 持久化

## 持久化的两种方式

- RDB：RDB 持久化机制，是对 Redis 中数据执行周期性的持久化
- AOF：AOF 机制对每条写入命令作为日志，以`append-only`模式写入一个日志文件中，在 Redis 重启的时候，额可以通过回放 AOF 日志中的写入指令来重新构建整个数据集

### RDB 的优缺点

- RDB 会生成多个数据文件，每个数据文件都代表了某一时刻中的 Redis 的数据，这种多个数据文件的方式，非常适合做冷备
- RDB 对 Redis 提供对外的读写服务，影响非常小，可以让 Redis 保持高性能，因为 Redis 只需要 fork 一个子进程，让子进程执行 IO 操作来进行 RDB 文件的持久化即可
- 相对于 AOF 持久化机制来说，直接基于 RDB 数据文件来重启和恢复 Redis 进程，更加快速
- 如果想要在 Redis 故障的时候，尽可能的少丢数据，RDB 没有 AOF 好
- RDB 每次在 fork 子进程来执行 RDB 快照数据文件生成的时候，如果数据文件特别大，可能会导致客户端提供的服务暂停数毫秒，或者甚至数秒

### AOF 的优缺点

- AOF 可以更好的保护数据不丢失，一般 AOF 会每隔 1 秒，通过一个后台线程执行一次`fsync`操作，最多丢失 1 秒钟的数据。
- AOF 日志文件以 `append-only` 模式写入，所以没有任何磁盘寻址的开销，写入性能非常高，而且文件不容易破损，即使文件尾部破损，也很容易修复。
- AOF 日志文件即使过大的时候，出现后台重写操作，也不会影响客户端的读写。因为在 `rewrite` log 的时候，会对其中的指令进行压缩，创建出一份需要恢复数据的最小日志出来。在创建新日志文件的时候，老的日志文件还是照常写入。当新的 merge 后的日志文件 ready 的时候，再交换新老日志文件即可。
- AOF 日志文件的命令通过非常可读的方式进行记录，这个特性非常**适合做灾难性的误删除的紧急恢复**。
- 对于同一份数据来说，AOF 日志文件通常比 RDB 数据快照文件更大。
- AOF 开启后，支持的写 QPS 会比 RDB 支持的写 QPS 低，因为 AOF 一般会配置成每秒 `fsync` 一次日志文件
- AOF 就是为了避免 rewrite 过程导致的 bug，因此每次 rewrite 并不是基于旧的指令日志进行 merge 的，而是**基于当时内存中的数据进行指令的重新构建**，这样健壮性会好很多。

### 选择

不要仅仅使用 RDB 或者 AOF，应该同时开启两种持久化方式

## Redis Cluster

### 介绍

- 自动将数据进行分片，每个 master 上放一部分数据
- 提供内置的高可用支持，部分 master 不可用时，还是可以继续工作

### 节点内通信协议

集群元数据的维护有两种方式：集中式、Gossip 协议。redis cluster 节点间采用 gossip 协议进行通信。

**集中式**：将集群元数据集中存储在某个节点上。其好处是在于元数据的读取和更新时效性非常好，一旦出现了数据变更，就立即更新到集中式存储中，其它节点读取的时候就可以感知到；不好在于，所有的元数据的更新压力全部集中在一个地方，可能会导致元数据的存储有压力。

**gossip**：所有节点都持有一份元数据，不同的节点如果出现了元数据的变化，就不断将元数据发送给其他节点，让其它节点也进行元数据的变更。其好处在于元数据从更新比较分散，不是集中在一个地方，更新请求会陆陆续续到达所有节点上更新，降低了压力；不好在于，元数据更新有延迟，可能导致集群中的一些操作会有一些滞后。

### 分布式寻址算法

### 哈希算法

首先计算 key 的 hash 值，然后对节点取模，然后进入不同的集群。一旦某一个节点宕机，所有请求都会基于最新的的剩余节点去取模，然后去取数据，然后这样会导致大部分的请求都不能拿到有效的缓存，然后导致大量的请求直接查询数据库

### 一致性哈希算法

一致性哈希算法就是讲整个哈希空间组织成一个虚拟的圆环，整个空间按顺时针方向组织，下一步将各个 master 节点进行哈希，这样就能确定

## 参考文档

1. [Distributed locks using Redis](https://engineering.gosquared.com/distributed-locks-using-redis)

# TODO

- [ ] gossip 协议