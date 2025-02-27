Union File System，简称 UnionFS 是一种为 Linux FreeBSD NetBSD 操作系统设计的，把其他文件系统联合到一个联合挂载点的文件系统服务。

它使用 branch 不同文件系统的文件和目录“`透明地`”覆盖，形成一个单一一致的文件系统。这些 branches 或者是 read-only 或者是 read-write 的。

将多个文件联合在一起成为一个统一的视图。

## 常见实现

- AUFS
- overlayfs