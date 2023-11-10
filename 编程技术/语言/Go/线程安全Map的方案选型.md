# 方案

1. 通过 map 数据类型 + 锁
2. 使用标准库内置的`sync.Map`对象
3. 分段锁

![[assets/74bf7fd25e401152f028da7fd90fcc74_MD5.jpeg]]

ref: 

- [Go 线程安全 map 方案选型 - 蛮荆](https://dbwu.tech/posts/golang_thread_safe_map/)
