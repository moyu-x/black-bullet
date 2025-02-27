# 出现 `java.lang.OutOfMemoryError: DIrect buffer memory`

ref: [错误：java.lang.OutOfMemoryError：DIrect 缓冲区内存 --- ERROR: java.lang.OutOfMemoryError: DIrect buffer memory](https://support.datastax.com/s/article/ERROR-java-lang-OutOfMemoryError-DIrect-buffer-memory)

对于特殊的对性能不高的 场景，可以考虑使用：

- `-Dio.netty.noPreferDirect=true`
- `-Dio.netty.maxDirectMemory=0`
- `-XX:MaxDirectMemorySize=500m`
- `-Xmx3g -Xms3g`