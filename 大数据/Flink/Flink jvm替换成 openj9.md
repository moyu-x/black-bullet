这里主要替替换成的是 1.8 的 openj9 jvm，所有需要需要修改配置

https://github.com/eclipse-openj9/openj9/issues/14950 这里下载补丁包

这里假设 jdk 的路径为`/opt/openj9`，并且`setvendor8.zip`也已经下载到这个目录中
# hardoop的修改

## 修改`hadoop-env.sh`

修改如下

```bash
export HADOOP_OPTS="$HADOOP_OPTS -javaagent:/opt/openj9/setvendor8.zip"
```

## 修改`yarn-env.sh`

在最后一行增加如下

```bash
YARN_OPTS="$YARN_OPTS -javaagent:/opt/openj9/setvendor8.zip"
```

# Flink的修改

修改`bin/config.sh`，在最开头增加

```bash
export JVM_ARGS="$JVM_ARGS -javaagent:/opt/openj9/setvendor8.zip"
```

然后在`conf/flink-conf.yaml`中增加如下

```yaml
env.java.opts: "-javaagent:/usr/java/jdk1.8.0_144/setvendor8.zip"
```


# ref

1. [hadoop使用openJ9报错：unable to find LoginModule class: com.ibm.security.auth.module.LinuxLoginModule解决_flink unable to find loginmodule class-CSDN博客](https://blog.csdn.net/applebomb/article/details/135060002)
2. [如何在启动taskmanager时传入自定义的java参数_env.java.opts.taskmanager-CSDN博客](https://blog.csdn.net/qq_26502245/article/details/108629000)
3. [openj9-0.32.0-m2 Linux x64 regression: No LoginModule found for com.ibm.security.auth.module.LinuxLoginModule · Issue #14950 · eclipse-openj9/openj9](https://github.com/eclipse-openj9/openj9/issues/14950)