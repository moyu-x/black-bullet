# 编译

```bash
# debian 和 Ubuntu 的依赖
sudo apt-get install ninja-build gettext cmake unzip curl build-essential

# redhat 系列的依赖
sudo dnf -y install ninja-build cmake gcc make unzip gettext curl glibc-gconv-extra

# 编译命令
git clone https://github.com/neovim/neovim

cd neovim && git checkout stable && make CMAKE_BUILD_TYPE=RelWithDebInfo
```
