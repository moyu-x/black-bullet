# 编译错误

## 出现`javax.annotation.meta.When`不存在

在配置文件中引入以下配置：

```groovy
compileOnly('com.google.code.findbugs:annotations:3.0.1')
```

# Tomcat

## Tomcat在Windows上出现乱码

参考：[https://blog.csdn.net/nan_cheung/article/details/79337273](https://blog.csdn.net/nan_cheung/article/details/79337273)

1. 将idea的FileEncoding设置为UTF-8，并设置 Transparent native-to-ascii
2. 将tomcat的启动参数增加`Dfile.encoding=UTF-8`
3. 在idea的`vm.propreties`中增加`Dfile.encoding=UTF-8`

## Lombok的问题

第一，由于Lombok使用比较简单，而且代码也变得简单，针对初学者容易只知其然，不知其所以然。注解用的很溜，但不明白或不清楚注解到底干了什么。

其实这是学习任何一项技术都会遇到的问题，建议在学习使用新技术、新框架时对自己要求严格那么一点点。

第二，@Data定义一个类的时候，会自动帮我们生成equals()方法。如果你对equals方法和hashcode方法有特殊的要求，建议自己手动实现。

如果实体类有继承关系，那么只使用@Data注解而不使用@EqualsAndHashCode(callSuper=true)的话，此时生成的equals方法只会比较子类的属性字段，不会考虑父类的继承属性。

## idea老版本

[https://www.jetbrains.com/idea/download/other.html](https://www.jetbrains.com/idea/download/other.html)

[JetBrains 2020.2 版本 全家桶激活方式.rar](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b7f5e69-a483-43bd-b3ab-e1cb7b5fa203/JetBrains_2020.2__.rar)

# 其他

[Maven](https://www.notion.so/Maven-8764db5679ae45a5bc83092e05f7afd2?pvs=21)

[JOOQ](https://www.notion.so/JOOQ-65c13ee0538b4129869e02da316e97ad?pvs=21)