# 第十章 集成Spring

## 集成

Spring Integration 支持创建集成流，应用程序可以通过这些集成流接收或发送数据到应用程序本身的外部资源。

声明集成流的三个配置选项包括：

- XML 配置
- Java 配置
- 使用 DSL 进行 Java 配置

## 组件

组件在集成流中所扮演的角色：

- _Channels_ —— 将信息从一个元素传递到另一个元素。
- _Filters_ —— 有条件地允许基于某些标准的消息通过流。
- _Transformers_ —— 更改消息值或将消息有效负载从一种类型转换为另一种类型。
- _Routers_ —— 直接将信息发送到几个渠道之一，通常是基于消息头。
- _Splitters_ —— 将收到的信息分成两条或多条，每条都发送到不同的渠道。
- _Aggregators_ —— 与分离器相反，它将来自不同渠道的多条信息组合成一条信息。
- _Service activators_ —— 将消息传递给某个 Java 方法进行处理，然后在输出通道上发布返回值。
- _Channel adapters_ —— 将通道连接到某些外部系统或传输。可以接受输入，也可以向外部系统写入。
- _Gateways_ —— 通过接口将数据传递到集成流。

### 消息通道

Spring Integration 提供了多个管道的实现，包括以下这些：

- PublishSubscribeChannel —— 消息被发布到 PublishSubscribeChannel 后又被传递给一个或多个消费者。如果有多个消费者，他们都将会收到消息。
- QueueChannel —— 消息被发布到 QueueChannel 后被存储到一个队列中，直到消息被消费者以先进先出（FIFO）的方式拉取。如果有多个消费者，他们中只有一个能收到消息。
- PriorityChannel —— 与 QueueChannel 类似，但是与 FIFO 方式不同，消息被冠以 priority 的消费者拉取。
- RendezvousChannel —— 与 QueueChannel 期望发送者阻塞通道，直到消费者接收这个消息类似，这种方式有效的同步了发送者与消费者。
- DirectChannel —— 与 PublishSubscribeChannel 类似，但是是通过在与发送方相同的线程中调用消费者来将消息发送给单个消费者，此通道类型允许事务跨越通道。
- ExecutorChannel —— 与 DirectChannel 类似，但是消息分派是通过 TaskExecutor 进行的，在与发送方不同的线程中进行，此通道类型不支持事务跨通道。
- FluxMessageChannel —— Reactive Streams Publisher 基于 Project Reactor Flux 的消息通道。