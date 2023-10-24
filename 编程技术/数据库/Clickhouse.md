# 数据删除

开始数据删除后短时间数据还存在，所以需要监测数据是否删除，然后才能进一步采取其他措施
# 参考文档

## 存算分离

- [ClickHouse 实现存算分离：与 Amazon S3 结合使用的三种方式 - 掘金](https://juejin.cn/post/7280007832714870796)

---


- 资源占用
	- 内存占用的估计需要考虑到`group by`、`distinct`、`join`及其他选项的使用
	- 数据存储体积估计
		- 数据量的大小
		- 数据的压缩系数
	- 对于分布式的部署，建议内部使用10G带宽的网络
- 存储引擎的选择
	- 怎样和哪里存储数据
	- 什么样的查询需要被支持
	- 数据是否需要存在副本
	- 大部分场景使用`MergeTree`
	- 对于需要去重的场景`ReplacingMergeTree`在合并的时候进行数据去重
	- 实时更新的场景使用
		- `CollapsingMergeTree`
			- 如果数据有相同的primary key和sort order key，存在不一致的数据的列，就插入+1状态的列然后抵消原有列
			- 被取消的列会在合并的时候被删除
			- 保留没有被处理的行
		- `VersionedCollapsingMergeTree`
			- 它删除每对具有相同主键和版本但符号不同的行
			- 插入行的顺序不是重要的
			- 如果version列不是主键的一部分，ClickHouse 会将其作为最后一个字段隐式添加到主键
- 主要特性
	- 追求高效的实时写入和交互式查询
	- 追求系统设计纯度
	- “白盒”计算模式
- 主键
	- **主键在clickhouse中对于每一行数据不是唯一的**
	- 主键决定了数据写入磁盘的方式
	- `primary key`和`order by`同时存在的时候，`primary key`必须是`order by`的子集
	- 排序顺序遵循左右先后
	- clickhouse在查询时候是否可以使用主键是影响查询的关键因素，对于有多个排序键的情况下，只使用第二个排序键去查询，此时排序键就失去了意义，会进行全表扫描
- [[Clickhouse的索引]]
- Clickhouse的materialized view
	- 问题
		- 仅支持一种排列方式
		- OLAP预聚合模型需要手动操作，需要指定指标、维度，然后查询根据指定好的进行改写，这样才能命中物化的结果
		- ClickHouse物化视图无一致性保证，在明细表里做聚合查询和在物化表里做聚合查询出来的结果可能不一样
- [[Clickhouse的projection]]
- 数据存储
	- KAFKA直接写入到clickhouse数据库中
		- ![image.png](../assets/image_1679629794405_0.png)
		- 如果从KAFKA消费的是JSON数据的时候，建议设置`input_format_skip_unknown_fields`，以避免字段不存在的错误
		- 考虑设置`kafka_skip_broken_messages`
		- 使用`libkafka`库
	- 插入数据
		- 每一批次可以插入大量数据
		- 使用`async_insert`配置
- 最佳实践
	- 在生产环境中关闭系统交换文件
	- 异步插入数据
		- 在默认情况下，clickhouse是同步写入数据的。设置`async_insert`为1启用异步写入，然后可以通过设置`async_insert_max_data_size`设置缓冲区的大小或者设置`async_insert_busy_timeout_ms`来配置写入的间隔
		- 在默认情况下，`wait_for_async_insert`设置为0，数据写入到buffer中就会返回确认，设置为1的时候要等到数据被写到存储上才会返回确认
		- 配置方式
			- 在用户层面进行更改，例如`ALTER USER default SETTINGS async_insert = 1`
			- 在插入语句前增加配置，如`INSERT INTO risk SETTINGS async_insert=1, wait_for_async_insert=0 VALUES (...)`
			- 在连接参数上进行配置
	- 避免修改数据，如果需要修改数据，不要使用`MergeTree`引擎，而是使`ReplacingMergeTree`或者`CollapsingMergeTree`
	- 避免出现可以为空的列，建议给字段加上默认值来避免此情况
	- 避免执行`OPTIMIZE TABLE ... FINAL`语句，此操作会将数据解压缩并且读出后再重新存储
	- 使用批量插入功能，默认情况每次插入都会创建一个存储部分，所以建议创建一个较大批次的数据，并且加上异步插入的功能。建议将插入查询控制在一定的速率，每秒发送过多的插入查询会导致后台的合并跟不上插入效率
	- 选择一个低离散度的列作为分区
	- 查看单条语句的执行情况:
		- ```bash
		  clickhouse-client --port 9002 --send_logs_level=trace <<< 'SQL 语句'
		  ```
- [[Clickhouse FAQ]]
- 参考资料
	- 优化
		- [The Secrets of ClickHouse Performance Optimizations](https://presentations.clickhouse.com/bdtc_2019/#cover)
		- [clickhouse到底有哪些吊炸天的优化？ - 知乎](https://www.zhihu.com/question/446288242)
	- 字符串优化
		- [Add StringHashMap to optimize string aggregation by amosbird · Pull Request #5417 · ClickHouse/ClickHouse](https://github.com/ClickHouse/ClickHouse/pull/5417)
	- 其他
		- ["Building for Fast" by Alexey Milovidov, Amsterdam, June 2022 - YouTube](https://www.youtube.com/watch?v=CAS2otEoerM)
		- [ClickHouse 查询优化详细介绍 - 古道轻风 - 博客园](https://www.cnblogs.com/88223100/p/ClickHouse-query-optimization-in-detail.html)