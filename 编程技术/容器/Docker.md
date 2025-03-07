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

![[assets/c8bcc7352234d82191794572bee30ad6_MD5.jpeg]]

方法二：连接到主机网络

```shell
docker run -d --network=host my-container:latest
```

方法三：使用默认桥接模式访问主机


## 镜像设置

### 安装仓库的镜像

在CentOS安装的过程中执行如下指令：

```bash
# 移除老版本数据
sudo yum remove docker \\
                  docker-client \\
                  docker-client-latest \\
                  docker-common \\
                  docker-latest \\
                  docker-latest-logrotate \\
                  docker-logrotate \\
                  docker-engine
# 设置仓库和镜像，替换为TUNA源
sudo yum install -y yum-utils
sudo yum-config-manager \\
    --add-repo \\
    <https://download.docker.com/linux/centos/docker-ce.repo>
sudo sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo

# 安装
sudo yum install docker-ce docker-ce-cli containerd.io
```

### 配置 Docker 为国内镜像

编辑`/etc/docker/daemon.json`文件后，然后重启`Docker`服务：

```json
{
  "insecure-registries": ["<https://registry.mirrors.aliyuncs.com>"],
  "registry-mirrors": ["<https://hub-mirror.c.163.com/>"]
}
```

## 添加用户到 Docker 用户组

在安装`Docker`之后，用如下指令将当前用户加入到用户组：

```bash
sudo usermod -aG docker $USER
```

如果提示`/var/run/docker.sock`权限不足，使用如下指令解决：

```bash
sudo chmod a+rw /var/run/docker.sock
```

## 在 mac 上访问宿主机

在`linux`可以使用`host`模式访问主机，但是在 mac 上要在镜像内访问主机，需要在使用 ip 的地方替换成`docker.for.mac.host.internal`，或者在执行`ifconfig`后，选择`docker0`网卡的 ip 地址

## 防火墙

### 解决 UFW 和 Docker 的问题

目前新的解决方案只需要修改一个 UFW 配置文件即可，Docker 的所有配置和选项都保持默认。修改 UFW 的配置文件 `/etc/ufw/after.rules`，在最后添加上如下规则：

```
# BEGIN UFW AND DOCKER
*filter
:ufw-user-forward - [0:0]
:DOCKER-USER - [0:0]
-A DOCKER-USER -j RETURN -s 10.0.0.0/8
-A DOCKER-USER -j RETURN -s 172.16.0.0/12
-A DOCKER-USER -j RETURN -s 192.168.0.0/16
-A DOCKER-USER -p udp -m udp --sport 53 --dport 1024:65535 -j RETURN
-A DOCKER-USER -j ufw-user-forward
-A DOCKER-USER -j DROP -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 192.168.0.0/16
-A DOCKER-USER -j DROP -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 10.0.0.0/8
-A DOCKER-USER -j DROP -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 172.16.0.0/12
-A DOCKER-USER -j DROP -p udp -m udp --dport 0:32767 -d 192.168.0.0/16
-A DOCKER-USER -j DROP -p udp -m udp --dport 0:32767 -d 10.0.0.0/8
-A DOCKER-USER -j DROP -p udp -m udp --dport 0:32767 -d 172.16.0.0/12
-A DOCKER-USER -j RETURN
COMMIT
# END UFW AND DOCKER
```

然后重启 UFW，`sudo systemctl restart ufw`。现在外部就已经无法访问 Docker 发布出来的任何端口了，但是容器内部以及私有网络地址上可以正常互相访问，而且容器也可以正常访问外部的网络。**可能由于某些未知原因，重启 UFW 之后规则也无法生效，请重启服务器。参考：**[https://github.com/chaifeng/ufw-docker](https://github.com/chaifeng/ufw-docker)

## 常用指令

### 文件拷贝

```bash
# 从容器里面拷文件到宿主机
# docker cp 容器名：要拷贝的文件在容器里面的路径 --> 要拷贝到宿主机的相应路径
docker cp testtomcat：/usr/local/tomcat/webapps/test/js/test.js /opt

# 从宿主机拷文件到容器里面
# 在宿主机里面执行如下命令
# docker cp 要拷贝的文件路径 容器名：要拷贝到容器里面对应的路径
docker cp /opt/test.js testtomcat：/usr/local/tomcat/webapps/test/js

# 查看容器名称,执行命令
docker ps
```

### 普通指令

```bash
# 杀死所有正在运行的容器
docker kill $(docker ps -a -q)

# 删除所有已经停止的容器
docker rm $(docker ps -a -q)

# 删除所有未打 dangling 标签的镜像
docker rmi $(docker images -q -f dangling=true)

# 删除所有镜像
docker rmi $(docker images -q)

# 为这些命令创建别名
# ~/.bash_aliases

# 杀死所有正在运行的容器.
alias dockerkill='docker kill $(docker ps -a -q)'

# 删除所有已经停止的容器.
alias dockercleanc='docker rm $(docker ps -a -q)'

# 删除所有未打标签的镜像.
alias dockercleani='docker rmi $(docker images -q -f dangling=true)'

# 删除所有已经停止的容器和未打标签的镜像.
alias dockerclean='dockercleanc || true && dockercleani'

# Docker提供了docker system prune，可以用于清理dangling镜像(参考What are Docker <none>:<none> images?)和容器，以及失效的数据卷和网络。
docker system prune

# 这个命令将清理整个系统，并且只会保留真正在使用的镜像，容器，数据卷以及网络
docker system prune -a

# 添加普通用户到Docker用户组（需要重启Docker）
sudo gpasswd -a ${USER} docker

# 提示socket权限不够
sudo chmod a+rw /var/run/docker.sock

# 清理
docker system prune --all
```

### 在重启docker的时候自动重启容器

在参数中加入`--restart=always`参数即可