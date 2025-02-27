# 支持[[达梦数据库]]

如果单机环境，可以考虑使用`embedded`模式，如果集群环境，可以考虑直接修改源码或者增加插件进行使用

## 对于 2.1 版本

这个版本没有多数据源插件，通过源码可以发现驱动是写死在代码中的，就配置中的`spring.datasource.platform=mysql`这个是不能改的，就是个摆设。。。

这个版本需要修改连接配置，增加驱动就可以了，可以参考后续给出的文档

每次编译后都需要把`distribution`下的`target`文件夹删了，再进行重新编译，使用打包命令如下：

```bash
mvn -Prelease-nacos -Dmaven.test.skip=true -Dcheckstyle.skip=true -Drat.skip=true -Dpmd.skip=true clean install -U
```

ref:
- [nacos 当前是否支持达梦等国产数据库？ · Issue #8280 · alibaba/nacos](https://github.com/alibaba/nacos/issues/8280)
- [nacos-plugin/nacos-datasource-plugin-ext/nacos-dm-datasource-plugin-ext at develop · nacos-group/nacos-plugin](https://github.com/nacos-group/nacos-plugin/tree/develop/nacos-datasource-plugin-ext/nacos-dm-datasource-plugin-ext)
- [如何让 Nacos 支持达梦数据库作为外置数据源 - 掘金](https://juejin.cn/post/7304263961243500578?from=search-suggest)
