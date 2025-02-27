*v1.21.4*

`sync/atomic`提供了原子同步操作原语，整个过程中无须加锁，也不会产生`goroutine`上下文切换

# API

![[assets/babc28d7ca1cf4d7e7662c3f726a4863_MD5.jpeg]]

# 函数实现

此类的实现是通过汇编实现在`asm.s`中，并且可以看到是通过`JMP`指令来进行实现

![[assets/32325c874b14fceffd77b1c7b7541014_MD5.jpeg]]

# atomic.Value

## Value 对象

源码最上面有几个使用的约束

![[assets/2046dab3eaaad9440560f5471d7dcfa4_MD5.jpeg]]

当时在内部数据调用时候，会使用`efaceWords`，其将类型和值分开进行处理

![[assets/5ba23e74254fa28fd47af298efa3bf4d_MD5.jpeg]]

## Value.Store

这里源码上有个限制，就是第一个传入的类型是什么，后续的类型就得是什么，参数不一致或者传递 nil 都会产生 panic。

并且可以看到是使用 pin 占用来实现 CPU 的抢占

![[assets/02467292087a4c748550e3d983d20b6a_MD5.jpeg]]


# 其他

官方给出的建议是除了特殊的、底层的应用程序外，其他情况最好使用 `channel` 或其他同步原语来完成

# ref

- https://dbwu.tech/posts/golang_atomic/


