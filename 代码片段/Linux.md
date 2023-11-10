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

# Docker

## 当前用户加入 docker 组

```bash
sudo usermod -aG docker ${USER}
```

## 配置镜像加速

https://gist.github.com/y0ngb1n/7e8f16af3242c7815e7ca2f0833d3ea6