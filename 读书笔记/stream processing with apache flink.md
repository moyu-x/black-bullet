# 基础

## Streaming architecutres

- Streaming Layers
- Stream Processing Enginers
- Realtime Analytical Data Stores
- Datalake

## Stream processing system should provide the following

- Grouping Aggregates
- support for different kinds of streaming joins
- adviced time windowing to perform computations
- support for watermarks for handling late-arriving events
- state management and the ability to handle large amounts of state
- provide exactly-once semantisc
- lower-level APIs that allow access to the system's internals like time and state

# flink

## flink job

![[assets/63b46a94f4d2fa5f4b456e6fef9736f1_MD5.jpeg]]

![[assets/12c38384eb35f7edbfc7dadbb85d777b_MD5.jpeg]]
![[assets/469668ddb923c75e9cc5b2bc20fb4d3d_MD5.jpeg]]

![[assets/d951ff336ba82145f0b3f82887833b73_MD5.jpeg]]
## data exchange strategies

- forward strategy
- broadcast strartegy
- random strategy
- key-based strategy

TaskManager 在同一个 JVM 进程中多线程地执行其任务。

## 影响性能的因素

- TaskManager 的大小
- 为每个算子设置并行度，覆盖任务或者集群的默认配置
- disable operator chaining
- using slot-sharing groups to force operators into their own slots

**一个 slot 可能会运行多个 task/threads **

## Flink SQL

![[assets/4153b305003a3c18fe13498220a5b0ef_MD5.jpeg]]
![[assets/32361fb8023c8ce058cb46eaad6a1af6_MD5.jpeg]]
## Streaming SQL 应该具有以下语义

1. 输入是不断变化的，并且是无限的
	1. 仅追加流
	2. Changelog Streams

## Flink window

- tumbling window
- sliding window
- comulative window
- session window

## 合并多个流

- Union Function
- Connect Function

# 容错

Flink 的恢复机制是基于 chandy-lamport 算法实现的

## 流处理系统提供三种数据处理语义

![[assets/7eb5595d8d4d0210c801dc7a1ba95851_MD5.jpeg]]

1. 至多一次
2. 至少一次
3. 精确一次

## 支持的重启策略

1. No restart
2. fixed delay restart
3. exponential delay restart
4. failure rate restart