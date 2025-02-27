Linux Namespace 是 Kernel 的一个功能，它可以隔离一系列的系统资源，比如 PID ( Process ID）、User ID、Network 等。

Namespace 的 API 主要使用如下 3 个系统调用

- `clone()`创建新进程。其本身是使当前进程加入到新的 Namespace
- `unshare()`将进程移出某个 Namespace，并加入到新创建的 Namespace 中。其是使当前的进程加入到新的 Namespace 中
- `setns()`将进程加入到 Namespace 中


## UTS Namespace

UTS Namespace 主要用来隔离 nodename 和 domainname 个系统标识。在 UT Namespace 里面，每个 Namespace 允许有自己的 hostname

## IPC Namespace

IPC Names 用来隔离 System V IPC  和 POSIX message queues。每一个 IPC Namespace 都有自己的 System V IPC  和 POSIX message queues

## PID Namespace

PID Namespace 是用来隔离进程 ID。同样一个进程在不同的 PID Namespace 里可有不同的 PID。

## Mount Namespace

Mount Namespace 用来隔离各个进程看到的挂载点视图。在不同 Namespace 的进程中到的文件系统层次是不 一样的。在 Mount Namespace 调用`mount()`和`umount()`仅仅只会影响当前 Namespace 内的文件系统，而对全局的文件系统是没有影响的。

## User Namespace

User Namespace 主要是隔离用户 用户组 ID 就是说 一个进程的 User ID 和 Group ID 在 User Namespace 内外是不同的

## Network Namespace

Network Namespace 是用来隔离网络设备、IP 地址端口等网络械的 Namespace Network。Namespace 可以让每个容器拥有自己独立的（虚拟的）网络设备，而且容器内的应用可以绑定到自己的端口，每个 Namespace 内的端口都不会互相冲突。在宿主机上搭建网桥后，就能很方便地实现容器之间的通信，而且不同容器上的应用可以使用相同的端口