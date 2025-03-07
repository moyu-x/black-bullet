# 创建 data 目录

```bash
sudo vim /etc/synthetic.conf

# 写入，中间这个是 tab 符号
data /Users/idwangmo/data
```

# 一些注意事项

1. 不要使用 brew 安装`wireshark`，会遇到`chmodbpf`无权限或者冲突的问题

## Mac 上不能发现`stdio.h`标准库

解决方案可以参考[macOS 软件编译时找不到头文件解决方法](https://zhile.io/2018/09/26/macOS-10.14-install-sdk-headers.html)这篇文章，具体的代码如下：

```bash
csrutil disable   # 需要在恢复模式下运行命令，具体请自行搜索。
xcode-select --install    # 安装常用开发工具，如：git 等。
sudo mount -uw /    # 根目录挂载为可读写，否则无法在/usr/下建立文件，本修改重启前有效。
sudo ln -s &quot;$(xcrun --show-sdk-path)/usr/include&quot; /usr/include
export SDKROOT=&quot;$(xcrun --show-sdk-path)&quot; # 设置环境变量
echo &quot;export SDKROOT=\\&quot;\\$(xcrun --show-sdk-path)\\&quot;&quot; &gt;&gt; ~/.bash_profile # zsh 的自行搞定
sudo DevToolsSecurity -enable # 将系统置于开发模式
```

## 高清 HDPI 的设置

从这个地方下载配置

[https://comsysto.github.io/Display-Override-PropertyList-File-Parser-and-Generator-with-HiDPI-Support-For-Scaled-Resolutions/注意：](https://comsysto.github.io/Display-Override-PropertyList-File-Parser-and-Generator-with-HiDPI-Support-For-Scaled-Resolutions/%E6%B3%A8%E6%84%8F%EF%BC%9A)

1. DisplayProductID
2. DisplayVendorID

这两个要配置正确，然后配合上 RDM 软件就可以享受高清的界面显示了