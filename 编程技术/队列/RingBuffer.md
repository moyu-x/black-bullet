优点：
- 环形数组结构：避免垃圾回收使用数组而非链表
- 元素位置定位：数组长度$2^{n}$，通过位运算，加快定位速度
- 无锁设计