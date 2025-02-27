# 复杂查询出现报错

可以修改配置文件中如下参数

![[assets/512ad319158f3c2c27e364372caee1c2_MD5.jpeg]]

# 提示 libunwind 问题

增加如下配置（对于后续的修复，此代码不在 2023.3 这个 lts 的分支上，除非升级到更新的)：

```xml
<clickhouse>
    <profiles>
        <default>
            <query_profiler_real_time_period_ns>0</query_profiler_real_time_period_ns>
            <query_profiler_cpu_time_period_ns>0</query_profiler_cpu_time_period_ns>
        </default>
    </profiles>
</clickhouse>
```

ref: https://github.com/ClickHouse/ClickHouse/issues/15638