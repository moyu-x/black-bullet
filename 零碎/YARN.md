# JDK的兼容性

| Hadoop/YARN Version | Supported JDK Versions | Recommended JDK | Notes            |
| ------------------- | ---------------------- | --------------- | ---------------- |
| 2.7.x               | 7, 8                   | 8               | End of Life      |
| 2.8.x               | 7, 8                   | 8               | End of Life      |
| 2.9.x               | 8                      | 8               | End of Life      |
| 3.0.x               | 8                      | 8               | Maintenance      |
| 3.1.x               | 8, 11                  | 8               | Production Ready |
| 3.2.x               | 8, 11                  | 11              | Production Ready |
| 3.3.0-3.3.3         | 8, 11                  | 11              | Production Ready |
| 3.3.4+              | 8, 11, 17              | 11              | Latest Version   |

# 出现java.lang.NoClassDefFoundError: org/apache/hadoop/yarn/api/ApplicationConstants

说明缺少hadoop yarn相关的依赖包，去上进行下载:

![[assets/6d87efd7ed4ed0bc5b767f59ca66796c_MD5.jpeg]]

