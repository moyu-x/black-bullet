# Vue

## history 模式下的配置

```
location / {
  try_files $uri $uri/ /index.html;
}
```

# 配置

## nginx client intended to send too large body

```
# 在http中加入
client_max_body_size 64M; #多少M根据实际情况填写
```

## Nginx DNS缓存问题

可以考虑在server的block中加入一个resolver，配置dns服务器地址，用此来解决域名后的服务器地址更换而nginx的解析不更新的问题

### 参考

[https://www.nadeau.tv/post/nginx-proxy_pass-dns-cache/](https://www.nadeau.tv/post/nginx-proxy_pass-dns-cache/)

## 配置提取参数

```
location ~ ^/mail-cloud/api/(.*) {
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass <http://127.0.0.1:3333/$1$is_args$args>;
}
```