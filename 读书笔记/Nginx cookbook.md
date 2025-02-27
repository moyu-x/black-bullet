使用`include`指令来引用配置，这样可以保持nginx配置的简洁清晰
```nginx
http {
  include conf.d/compossiong.conf;
  include ssl_conf/*.conf;
}
```

NGINX的负载均衡方式：

1. 轮询
2. 最少连接（连接少不代表响应快）
3. 最短时间
4. 通用哈希
5. 随机
6. IP哈希
