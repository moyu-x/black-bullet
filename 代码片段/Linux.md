# 用户

## 创建一个 wheel 用户

```bash
useradd -m -G wheel -s /bin/bash testuser  #wheel附加组可sudo，以root用户执行命令 -m同时创建用户家目录
```

# DNF

## 增加一个 repo

```bash
# dnf config-manager --add-repo _repository_URL_
```

## SHA-1 的包不能安装

ref: https://www.redhat.com/en/blog/rhel-security-sha-1-package-signatures-distrusted-rhel-9

执行如下命令

```bash
update-crypto-policies --set DEFAULT:SHA1

# 在包安装完成后，执行如下
update-crypto-policies --set DEFAULT
```
# Docker

## 当前用户加入 docker 组

```bash
sudo usermod -aG docker ${USER}
```

## 配置镜像加速

https://gist.github.com/y0ngb1n/7e8f16af3242c7815e7ca2f0833d3ea6

# Debian

## 增加 sudo 用户

Debian 12 对最简安装的用户管理进行了修改，所以可以根据如下方式进行增加

```bash
# 先下载sudo
apt install sudo

# 设置权限
/usr/sbin/usermod -aG sudo ${USER}

# 刷新权限
getent group sudo
```

## 对于想查找不存在的命令

```bash
sudo apt install command-not-found
```

## bash 补全位置

`/usr/share/bash-completion/completions/`