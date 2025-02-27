# 单机情况下

在单机 Redis 的情况下，可以使用如下方式实现

```
SET resource_name my_random_value NX PX 30000
```

或者使用`lua`脚本完成

```lua
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
```

# 集群环境下

[[RedLock]]