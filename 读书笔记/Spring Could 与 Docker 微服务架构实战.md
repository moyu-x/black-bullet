# 架构

单体架构存在的问题：

- 复杂性搞
- 技术债务
- 部署频率低
- 可靠性差
- 扩展能力受限
- 阻碍技术创新

微服务应该具备的特性：

- 每个微服务独立运行在自己的进程中
- 一系列独立运行的微服务共同构建起整个系统
- 每个服务为独立的业务开发，一个微服务只关注某个特定的功能
- 微服务之间通过一些轻量级通信机制通信
- 可以使用不同语言与数据存储技术
- 全自动的部署机制

微服务架构的优点：

- 易于开发和维护
- 单个微服务启动较快
- 局部修改容易部署
- 技术栈不受限
- 按续伸缩

微服务架构的挑战：

- 运维要求较高
- 分布式固有的复杂性
- 接口调整成本高
- 重复劳动

微服务设计的原则：

- 单一职责原则
- 服务自治原则
- 轻量级通信机制
- 微服务粒度

## Spring Cloud

硬编码的问题：

- 适用场景有局限
- 无法动态伸缩

## 服务注册与发现

服务通过者、服务消费者、服务发现组建三者之间的关系：

- 各个微服务在启动时，将自己的网络地址等信息注册刀服务发现组件中，服务发现组建会存储这些信息
- 服务消费者可以从服务发现组建查询服务提供者的网络地址，并适用该地址调用服务提供者的接口
- 各个微服务与服务发现组件适用一定机制通信。服务发现组建如长时间无法与某微服务实例通信，就会注销该实例

服务发现组建应该具备的功能：

- 服务注册表
- 服务注册与服务发现
- 服务检查

在 Spring Cloud 中，Feign 默认设计用的契约是`SpringMvcContract`

## Hystrix

Histrix 通过以下几点实现延迟和容错：

- 包裹请求
- 跳闸机制
- 资源隔离
- 监控
- 回退机制
- 自我修复

Hystrix 的隔离策略有两种：

- THREAD（线程隔离）
- SEMAPHORE（信号量隔离）

## 网关

微服务网关的优点：

- 易于监控
- 易于认证
- 减少了客户端与各个微服务之间的交互次数

Zuul 核心是过滤器，这些过滤器可以完成以下功能：

- 身份认证与安全
- 审查与监控
- 动态路由
- 压力测试
- 负载分配
- 静态响应处理
- 多区域弹性

Zuul 过滤器的生命周期

- PRE：在路由被请求之前调用
- ROUTING：过来器将请求路由刀微服务
- POST：路由刀微服务后执行
- ERROR：在其他阶段发生错误时执行该过滤器

## 配置中心

微服务的配置管理一般有以下需求：

- 集中配置管理
- 不同环境不同配置
- 运行期间可动态调整
- 配置后可自动更新

## 链路追踪

分布式计算的八大误区：

- 网络可靠
- 延迟为零
- 带宽无线
- 网络绝对安全
- 网络拓扑不会改变
- 必须有一名管理员
- 传输成本为零
- 网络同质化