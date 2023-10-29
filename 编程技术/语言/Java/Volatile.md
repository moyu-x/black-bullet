# 概念

- 保证内存可见性
- 防止指令重排
- 但是不能保证线程安全，只能保证这个变量访问是原子性，但是不能保证这个代码块是原子性的

# 分析

有如下代码

```java
package temp;

public class VolatileTest { 
    static volatile String key; 
    public static void main(String[] args){ 
        key = "Hello world"; 
    } 
}
```

使用`javap`命令查看字节码，可以看到如下内容：

```
  static volatile java.lang.String key;
    descriptor: Ljava/lang/String;
    flags: (0x0048) ACC_STATIC, ACC_VOLATILE
```

可以看到是在转换成字节码的时候在变量上增加了`ACC_VOLATITLE`表示来保证内存可见性的