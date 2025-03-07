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