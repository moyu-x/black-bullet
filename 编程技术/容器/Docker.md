# 概念

## 三个特点

- 轻量级
- 开放
- 安全

## 容器和虚拟机的比较

- 容器和虚拟机同样有着资源隔离和分配的优点，但是由于其架构的不同，容器比虚拟机更加便携和高效
- 虚拟机包含用户的程序，必要的函数库和整个客户操作系统
- 容器包含用户的程序和所有的依赖，但是容器之间是共享 kernel 的。各个容器在宿主机上互相隔离，并且在用户态下运行。

## Docker 是如何使用 AUFS 的

每一个 Docker image 都是由一系列 read-only layer 组成的。image layer 的内 容都存储在 Docker hosts filesystem 的/var/lib/docker/aufs/diff 目录下。而/var/lib/docker/aufs/layers 目录则存储着 image ayer 如何堆找这些 layer metadata

## 容器中访问宿主机

方法一 （只有在 docker desktop 版本上支持）：

![[assets/8cdddf75403bdeafb8d2cfbb6e532956_MD5.jpeg]]

方法二：连接到主机网络

```shell
docker run -d --network=host my-container:latest
```

方法三：使用默认桥接模式访问主机
