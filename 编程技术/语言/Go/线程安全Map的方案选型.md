# 方案

1. 通过 map 数据类型 + 锁
2. 使用标准库内置的`sync.Map`对象
3. 分段锁

![[assets/74bf7fd25e401152f028da7fd90fcc74_MD5.jpeg]]

# 哈希冲突处理

1. 链表数组法：每个键值对表长取模，如果结果相同，用链表的方式依次往后插入
2. 开放地址法
	1. 线性探测：一旦发生冲突，就把位置往后加 1，直到找到一个空的位置
	2. 平方探测：平方探测的发生冲突以后添加的值为查找次数的平方
	3. 双哈希探测：



ref: 

- [Go 线程安全 map 方案选型 - 蛮荆](https://dbwu.tech/posts/golang_thread_safe_map/)
