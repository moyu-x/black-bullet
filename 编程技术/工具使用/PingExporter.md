用于监控服务器时间的网络延迟，测试后发现，服务器禁ping 的情况下就会不能使用，所以一定要部署在防火墙后，并且服务器要能允许 icmp 协议

下载地址：

[czerwonk/ping_exporter: Prometheus exporter for ICMP echo requests using https://github.com/digineo/go-ping](https://github.com/czerwonk/ping_exporter)

使用方式：

```bash
ping_export www.baidu.com
```

这样可以启动，或者是直接部署成system'd
