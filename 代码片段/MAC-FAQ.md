# 创建 data 目录

```txt
sudo vim /etc/synthetic.conf

# 写入，中间这个是 tab 符号
data /Users/idwangmo/data
```

# 一些注意事项

1. 不要使用 brew 安装`wireshark`，会遇到`chmodbpf`无权限或者冲突的问题