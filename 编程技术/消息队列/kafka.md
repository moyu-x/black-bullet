主要设计目标：

- 以时间复杂度$O(1)$的方式提供消息持久化能力
- 高吞吐率
- 支持Kafka Server间的消息分区，及分布式消费
- 同时支持离线数据处理和实时数据处理。
- 在线水平扩展

业务上Kafka的一些作用：

- 能有效的隔离上下游业务

# 过期时间

一、kafka 全局消息过期时间设置 1） 进入kafka配置文件夹

cd  /opt/kafka/config/

默认的是在server.properties 文件里面

2）需要修改和配置项如下：

log.retention.hours=168 (配置该参数即可) log.cleanup.policy=delete （默认，可不配置）

3） 修改配置后重启kafka服务生效，kafka默认的消息过期时间为168h(7天)

该种设置消息过期时间的优点是可以对所有topic全部生效，缺点是需要重启kafka服务，造成服务短暂的不可用！

二、针对特定topic设置过期时间 实际上，可以不停服针对kafka中数据量较大的topic可以单独设置过期时间，不受全局过期时间的限制，而且不需要重启kafka

下面以数据较多的 mytopic为例， 全局默认消息过期时间为7天，现在将其调整为1天

1） 进入kafka 的安装目录

cd /opt/kafka/bin

1. 执行设置命令

./kafka-configs.sh --zookeeper localhost:2181 --alter --entity-name mytopic--entity-type topics --add-config retention.ms=86400000

时间设置一天 86400000 = 1天