# 三要素

分布式系统的三个指标，即 CAP 定理的三要素：

- 一致性（Consistency）
- 可用性（Availability）
- 分区容忍性（Partition Tolerance）

# 限制

![[assets/87056be8ca0111c492a619c26c9d6535_MD5.jpeg]]


CAP 定理指的是，这三个要素中最多只能同时实现两点，不可能三者兼顾。CAP 理论一般只能 AP 或 CP，而 CA 一般较难实现。

- CP: 要实现一致性，则需要一定的一致性算法，一般是基于多数派表决的，如 Paxos 和 Raft
- AP: 要实现可用性，则要有一定的策略决议到底用哪个数据，并且数据一般要进行冗余备份（replication），比如 Redis 集群中使用的 [Gossip 协议](https://en.wikipedia.org/wiki/Gossip_protocol)


