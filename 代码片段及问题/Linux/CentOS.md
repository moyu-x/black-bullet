# 软件安装

## FFmpeg 安装

1. 安装rpmfusion源
    
    ```bash
    sudo dnf install --nogpgcheck <https://mirrors.rpmfusion.org/free/el/rpmfusion-free-release-8.noarch.rpm> <https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-8.noarch.rpm>
    ```
    
2. 执行后续步骤
    
    ```bash
    sudo dnf config-manager --enable PowerTools
    sudo dnf update
    ```
    
3. 执行FFmpeg安装
    
    ```bash
    sudo dnf install ffmpeg
    ```
    

## Htop的编译安装

1. `dnf install ncurses-devel automake autoconf gcc clang`安装依赖
2. `wget <https://github.com/htop-dev/htop/releases/download/3.1.2/htop-3.1.2.tar.xz`下载最新安装包>
3. `tar -xf htop-3.1.2.tar.xz`进行解压
4. `cd htop-3.1.2 && ./autogen.sh`
5. `./configure CC=clang && make && make install`进行安装

# 软件源

## 更换软件源

```bash
sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \\
         -e 's|^#baseurl=http://mirror.centos.org/$contentdir|baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos|g' \\
         -i.bak \\
         /etc/yum.repos.d/CentOS-Base.repo \\
         /etc/yum.repos.d/CentOS-Extras.repo \\
         /etc/yum.repos.d/CentOS-AppStream.repo
```

## Centos-Stream-9 安装epel

```bash
dnf config-manager --set-enabled crb
dnf install \\
    <https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm> \\
    <https://dl.fedoraproject.org/pub/epel/epel-next-release-latest-9.noarch.rpm>
```

参考：[https://docs.fedoraproject.org/en-US/epel/#_el9](https://docs.fedoraproject.org/en-US/epel/#_el9)