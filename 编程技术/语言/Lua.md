# Centos安装luarocks

```bash
# 先安装相关依赖，全程使用luajit兼容的5.1
dnf install lua5.1 lua5.1-devel luajit

# 安装luarocks用于解决lua包管理
wget <https://luarocks.org/releases/luarocks-3.3.1.tar.gz>
tar zxpf luarocks-3.3.1.tar.gz
cd luarocks-3.3.1
./configure --with-lua-include=/usr/include/lua-5.1/ --lua-version=5.1
./configure --with-lua-include=/usr/include/luajit-2.1/ --lua-version=5.1
make
make install

# 设置默认的编译为lua 5.1
luarocks-admin --lua-version 5.1

# 安装依赖包
luarocks install net-url
luarocks install base64
luarocks install bit32
luarocks install lua-cjson

# ossl强烈依赖于系统的OpenSSL库，尽量使用系统自带的库而不是自己编译安装
dnf install lua5.1-luaossl

# 获取环境变量
luarocks path
```

# 测试

使用参考文档1的方式进行测试，可以实现suricata加载lua脚本，然后连上redis，并使用cjson

![[assets/8e88ee77df3a2f266bcc26d4ebf9234e_MD5.jpeg]]

# lua

## lua md5

a - z 是 97 - 122
0 - 9 是 48 - 57

# 文档参考

1. [https://www.freebuf.com/sectool/218951.html](https://www.freebuf.com/sectool/218951.html)
2. [https://wiki.tophant.com/pages/viewpage.action?pageId=55959322](https://wiki.tophant.com/pages/viewpage.action?pageId=55959322)
3. [https://suricata.readthedocs.io/en/suricata-6.0.0/lua/lua-functions.html#lua-functions](https://suricata.readthedocs.io/en/suricata-6.0.0/lua/lua-functions.html#lua-functions)
4. [LUA简明教程-CoolShell](https://coolshell.cn/articles/10739.html)
5. [Luajit ffi](https://luajit.org/ext_ffi.html)