### 源配置

编辑 /etc/pacman.d/mirrorlist， 在文件的最顶端添加： `Server = <https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch`更新软件包缓存：> `sudo pacman -Syy`

### ArchLinuxCN

使用方法：在 `/etc/pacman.conf` 文件末尾添加以下两行：

```
[archlinuxcn]
Server = <https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch>
```

之后安装 `archlinuxcn-keyring` 包导入 GPG key。

### 安装 base-devel

这个是在编译时候使用的一些基本的开发工具，使用如下代码进行安装：

```
sudo pacman -S base-devel
```

### 安装 linux-header

```
# 查询可用的headers信息sudo -Ss linux-headers# 查询内核信息uname -a# 最后结合两者的信息进行安装
```

### 初始化 key

```
sudo pacman-key --initsudo pacman-key --populate
```

### WSL 中设置默认用户

在`Powershell`中执行如下指令:

```
Arch.exe config --default-user {username}
```

## 用户管理

### 新建用户

这里以新建`wheel`为例，首先编辑`/etc/sudoer`文件，开启`wheel`用户组:

```
%wheel      ALL=(ALL) ALL
```

接下来执行建立用户操作:

```bash
useradd -m -G wheel -s /bin/bash {username}
passwd {username}
```

### 彻底删除一个用户

```
userdel -r username
```

# 在`WSL`中使用`pipenv`报错

出现在`wsl`中调用`pipenv`出现调用`Windows`中的`Python`的情况时候，需要在使用`pipenv`的时候指定`Python`版本的情况：

```
pipenv --python /usr/bin/python
```