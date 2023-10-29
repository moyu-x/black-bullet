# 使用

| 类型   | 代码示例                                   | 锁住的对象           |
| ---- | -------------------------------------- | --------------- |
| 普通方法 | synchronized void test() { }           | 当前对象            |
| 静态方法 | synchronized static void test() { }    | 锁的是当前类的Class 对象 |
| 同步块  | void fun () { synchronized (this) {} } | 锁的是（）中的对象       |

# 原理

有如下代码

```java
public class VolatileTest {
    public static void main(String[] args){
        synchronized (VolatileTest.class) {
            System.out.println("hello world");
        }
    }
}
```

通过`javap`查看字节码后

![](assets/ea849d049c2f0b245262a2b8e66935c6_MD5.jpeg)

可以看到，其是通过`Monitor`对象进行获取的，在获取代码前插入`monitorenter`，退出代码或者异常的位置增加`monitorexit`，对于没有获取到锁的对象，会一直阻塞执行代码，直到锁获取。

两个`monitorexit`的原因是会添加一个隐式的`try-final`，在`final`中增加指令防止锁释放不成功

# 几种状态

## 偏向锁

锁偏向某一个线程，此用来处理同一个线程多次获取一个锁的情况。但是出现多个线程获取同一个锁，就会升级变成轻量级锁

## 轻量级锁

基于CAS实现，当获取失败后，会进入一段时间的等待自选操作然后再获取锁，多次重试未成功就会升级为重量级锁

### 重量级锁

是基于底层操作系统实现的，每次获取锁失败都会直接让线程挂起，这会带来`用户态`和`内核态`的切换，性能开销比较大

# 其他用处

- 获得同步锁
- 清空工作内存
- 从主内存中拷贝对象副本到本地内存
- 执行代码
- 刷新主内存数据
- 释放同步锁

ref: https://juejin.cn/post/6844904155069284359