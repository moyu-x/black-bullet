# 基础

其利用了[RingBuffer](RingBuffer.md)、CAS 等高效的并发策略

# 一些设计理念

1. 零阻塞：采用了无所的设计
2. 预分配数据：通过将数据预先分配在 RingBuffer 中，使得每个处理之在处理数据时，数据在多个 CPU 之间的传递被降到最低


ref:

1. [并发编程 | 并发编程框架 - Disruptor - 深入理解高性能异步处理框架 - 掘金](https://juejin.cn/post/7295252900115251240)
