# 安装

```bash
go install github.com/ofabry/go-callvis@latest
```

# 使用

```bash
# 存在main函数
go-callvis .

# 使用test类
go-callvis --tests .
```

会生成如下的图

![[assets/9327fccd6a54d3083fb65b88c4dc4c2a_MD5.jpeg]]
# 评价

目前用下来感觉会比较鸡肋，不能对没有 test 或者 main 的代码进行分析，也就是不能对一些 lib 库（官方）进行分析

但是对一些业务代码的分析比较有用