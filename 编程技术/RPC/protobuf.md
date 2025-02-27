# 一些注意点

1. 在同一个 proto 文件中写多个 enum，其中的枚举值的名称不能重复
2. 并且为了 proto enum 和 Java 进行兼容，最好的方式就是第一个 key 默认为`ununsed = 0;`

protobuf官方没有对 c 语言进行支持，所以需要使用第三方库[protobuf-c/protobuf-c: Protocol Buffers implementation in C](https://github.com/protobuf-c/protobuf-c)

# ref

- [书写 .proto 文件规范](https://www.zhangjiee.com/topic/grpc/write-proto-spec.html)
- [proto3 默认值与可选项 - Tech For Fun](https://kaelzhang81.github.io/2017/05/15/proto3%E9%BB%98%E8%AE%A4%E5%80%BC%E4%B8%8E%E5%8F%AF%E9%80%89%E9%A1%B9/)
- [[转]Protobuf3 语法指南](https://colobu.com/2017/03/16/Protobuf3-language-guide/)
- [Protocol Buffers: Benchmark and mobile - OCTO Talks !](https://blog.octo.com/protocol-buffers-benchmark-and-mobile)
 