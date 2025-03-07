## 端口占用

1. [Linux 查看端口占用情况](https://www.runoob.com/w3cnote/linux-check-port-usage.html)

## 查看硬盘使用

```bash
du --max-depth=1 -B M
```

## nohup

```yaml
nohup command >/dev/null 2>&1 &
```

## zip

```bash
zip -r myfile.zip ./*
```

## 网络问题

在网络硬件设备工作正常的情况下，首先检查`/etc/resolv.conf`文件，如果为空或者配置错误就就行更新

## 关闭`tab`补全的警报

修改`/etc/inputrc`文件，将`bell-style`设置为如下：

```
set bell-style none
```

## 图片压缩

先安装`jpegiptim`工具：

```
sudo pacman -S jpegoptim
```

具体的使用方式如下：

```
jpegoptim example.jpeg# 需要压缩的情况jpegoptim --size=100k example.jpeg
```

## SSH

### 提示 `Broken pip`

这个时候在`~/.ssh/config`中加入如下的配置：

```
Host *  ServerAliveInterval 120  IPQoS=throughput
```

### 提示 `PTY allocation request failed on channel 0`

`PTY`是一个伪终端，提示为伪终端分配失败，然后提示成功后输出欢迎信息，最后连接被关闭了。这是因为基于`SSH`的`Git`不需要一个 tty，所以被拒绝分配成一个`tty`接入站点，这个时候使用`ssh -T`就行

### Git 密码管理

在使用 Git 的时候，会遇到一些只能使用用户名和密码进行代码下载的情况，尤其是在使用`Git Submodule`的时候，频繁的输入密码可以通过一下方式进行解决：

1. 在`home`目录创建`.netrc`文件
2. 在创建好到文件中输入并替换相关内容：

```
machine example.org
    login your-username
    password your-password
```

### SSH 自动退出

修改`/etc/ssh/sshd_config`文件中`ClientAliveCountMax`的参数，单位为分钟

### Git下SSH设置代理

在`~/.ssh/config` 文件中加入如下配置就行：

```bash
# 这里的 -a none 是 NO-AUTH 模式，参见 <https://bitbucket.org/gotoh/connect/wiki/Home> 中的 More detail 一节
ProxyCommand connect -S 127.0.0.1:1080 -a none %h %p
```

## 常用指令

### du

### 查看当前子文件夹的大小

```
du -h --max-depth=1
```

## OpenRestry & Nginx

### 增加`conf.d`配置

在`/etc/nginx.conf`中的`http`块下加入如下配置:

```
include /etc/nginx/config.d/*.conf;
```

### 基本配置

```
server {
    listen 80;
    server_name consul.idwangmo.top;

    location / {
        proxy_pass <http://localhost:8500>;
    }
}
```

## 线上环境出现too many open files

参考资料：[https://blog.51cto.com/mapengfei/4793701](https://blog.51cto.com/mapengfei/4793701)