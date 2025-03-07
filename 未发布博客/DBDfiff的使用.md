`DBDiff`是一个数据库对比并生成差异语句的一个工具，可以结合`Flyway`进行数据库的版本迁移使用。

## 安装

### 下载源码

```bash
git clone <https://github.com/DBDiff/DBDiff.git>
```

### 构建

```bash
# 安装 composer
curl -sS <https://getcomposer.org/installer> | php

# 设置源为阿里源
php composer.phar config repo.packagist composer <https://mirrors.aliyun.com/composer/>

# 进行构建
php composer.phar install
```

## 使用

### 配置文件

在`DBDiff`的目录里面创建配置文件，一个示例配置在`.dbdiff.example`中。在正常使用中，可以直接使用如下的配置：

```
server1:
    user: root
    password: 数据库密码
    port: 3306
    host: 127.0.0.1
server2:
    user: root
    password: 数据库密码
    port: 3306
    host: 127.0.0.1
type: schema
include: all
nocomments: true
```

### 执行命令

```bash
./dbdiff server1.数据库名称:server2.数据库名称 --output=simple-schema.sql
```

最终会生成一个`simple-schema.sql`的文件，里面分为两部分，`up`是`server2`相对于`server1`的区别，`down`是`server1`相对于`server2`的区别