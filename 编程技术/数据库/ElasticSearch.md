# 基础

## 提供了什么

1. 自动维护数据的分布到多个节点的索引建立、检索请求分布到多个节点的执行
2. 自动维护数据的冗余副本，保证一些机器宕机了，不会丢失任何数据
3. 封装了更多的高级功能

## 干什么

1. 分布式的搜索引擎和数据分析引擎
2. 全文检索，结构化检索，数据分析
3. 对海量数据进行近乎实时的处理

其本身屏蔽了`lucene`的一些复杂 API 调用

## 集群健康状态

- green：每个索引的 primary shard 和 replica shard 都是 active 状态的
- yellow：每个索引的 primary shard 都是 active 状态的，但是部分 replica shard 不是 active 状态，处于不可用的状态
- red：不是所有索引的 primary shard 都是 active 状态的，部分索引有数据丢失了

## master 节点

1. 创建或删除索引
2. 增加或删除节点

## 节点对等的分布式架构

1. 节点对等，每个节点都能接收所有的请求
2. 自动请求路由
3. 响应收集

## shard & replica 机制再次梳理

1. index 包含多个 shard
2. 每个 shard 都是一个最小工作单元，承载部分数据，是一个 lucene 实例，完整的建立索引和处理请求的能力
3. 增减节点时，shard 会自动在 nodes 中负载均衡
4. primary shard 和 replica shard，每个 document 肯定只存在于某一个 primary shard 以及其对应的 replica shard 中，不可能存在于多个 primary shard
5. replica shard 是 primary shard 的副本，负责容错，以及承担读请求负载
6. primary shard 的数量在创建索引的时候就固定了，replica shard 的数量可以随时修改
7. primary shard 的默认数量是 5，replica 默认是 1，默认有 10 个 shard，5 个 primary shard，5 个 replica shard
8. primary shard 不能和自己的 replica shard 放在同一个节点上（否则节点宕机，primary shard 和副本都丢失，起不到容错的作用），但是可以和其他 primary shard 的 replica shard 放在同一个节点上

对于 shard 和 replica 的总结：

1. pri : primary shard
2. rep : replica shard
3. 所以一个 es 实例叫做 shard
4. rep 的配置是针对于每个 pri 的副本个数

## 文档系统

### 元数据

1. `_index`
	1. 类似一个数据库
	2. 索引名必须小写，不能用下划线开头，不能包含逗号
2. `_type`
	1. 可以看成一个数据库表
	2. 一个索引通常会划分为多个 type，逻辑上对 index 中有些许不同的几类数据进行分类
	3. 名称可以是大写或者小写，但是同时不能用下划线开头，不能包含逗号
3. `_id`
	1. `document`的唯一标识

## document id

1. 手动指定：如果数据中存在唯一值的情况，可以直接使用这个为`document id`
2. 自动生成：
	1. 长度为 20 字符
	2. URL 安全：因为经过了 base64 编码
	3. GUID 方式

## 一些操作

### 限定字段

`_source`可以指定返回的字段

### CRUD

数据的修改是全量替换：

1. 和创建语句一致，`document id`存在则全量替换，不存在则新建
2. `doucment`是不可变的
3. 标记-->替换-->删除

### `bulk`

其会将数据加载到内存中，如果太大，性能反而会下降，因此需要反复尝试得到一个最佳的`bulk size`。一般 1000 到 5000 条开始尝试，如果看大小，最好 5 到 15MB 之间

## 数据控制

ES 内部使用基于`_version`的[[乐观锁]]机制进行并发控制

可以自定义`external version`来进行乐观锁的并发控制，此不属于 ES 控制，所以需要显示的大于已经存在的值

# 容错

## 机制

1. master node 宕机，自动 master 选举，red
2. replica 容错：新 master 将 replica 提升为 primary shard，yellow
3. 重启宕机 node，master copy replica 到该 node，使用原有的 shard 并同步宕机后的修改，green

# ref

1. https://zq99299.github.io/note-book/elasticsearch-core