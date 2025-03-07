# 基础

## 汇编知识

- Go 本身的汇编语言有很浓的 Plan 9 风格
- 伪寄存器是 Plan9 伪汇编的一个助记符
- 组合而不是继承

## 为什么没有 goroutine ID

![Open: Pasted image 20231108150019.png](assets/fb52eb4f135744a71f759fa524c08a87_MD5.jpeg)

## 编译速度快的原因
ref: https://dbwu.tech/posts/golang/why_golang_compiles_so_fast/
1. 极简关键字
2. 无符号表
	1. 是编译器中的一种数据结构，用于存储程序中的标识符
	2. Go 中并没有直接（显示）使用符号表，编译器会在编译过程中创建一些内部数据结构来管理标志符和类型信息
3. 依赖分析
	1. 消除依赖循环
	2. 依赖简化
4. 不支持重载/重写
5. 不支持模板
6. 无须虚拟机
7. 其他
	1. 所有导入的必须在源文件开头列出，所以编译器不必读取整个文件来确定依赖关系
	2. 编译后的 Go 对象文件不仅记录了包本身的导出信息，还记录了它的依赖

# 内存

内存分配使用 TCMalloc 算法

## 可能产生内存泄露的两个场景

- cgo 模块内存泄露
- 资源句柄未正常管理导致资源未释放

# 其它

在使用目前公司流量解析后数据，统一进行性能测量，目前可以看出性能最好的是 sonic

![[Pasted image 20230802113655.png]]

不要完全相信 revcover，它不能恢复 runtime 的一些 painc