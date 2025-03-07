# 混淆(Proguard)

## 基本概念

ProGuard能够对Java类中的代码进行压缩（Shrink）,优化（Optimize）,混淆（Obfuscate）,预检（Preveirfy）。

1. 压缩（Shrink）:在压缩处理这一步中，用于检测和删除没有使用的类，字段，方法和属性。
    
2. 优化（Optimize）:在优化处理这一步中，对字节码进行优化，并且移除无用指令。
    
3. 混淆（Obfuscate）:在混淆处理这一步中，使用a,b,c等无意义的名称，对类，字段和方法进行重命名。
    
4. 预检（Preveirfy）:在预检这一步中，主要是在Java平台上对处理后的代码进行预检。
    

![](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3c58d7e4-ae1f-4aa6-87de-d0b0333e17e8/Untitled.png)

## 使用

**代码使用`rule-Agent`的代码作为示例，代码在`feature/proguard`分支**

在`pom.xml`文件中的`build.plugins`块下加入如下的配置，这个配置得根据每个项目进行详细并且反复的测试才行，不是通用的配置代码：

```xml
<plugin> 
  <groupId>com.github.wvengen</groupId>  
  <artifactId>proguard-maven-plugin</artifactId>  
  <version>2.3.1</version>  
  <executions> 
    <execution> 
      <phase>package</phase>  
      <goals> 
        <goal>proguard</goal> 
      </goals> 
    </execution> 
  </executions>  
  <configuration> 
    <proguardVersion>6.2.2</proguardVersion>  
    <injar>${project.build.finalName}.jar</injar>  
    <outjar>${project.build.finalName}.jar</outjar>  
    <obfuscate>true</obfuscate>  
    <options> 
      <!--指定java版本号-->  
      <option>-target 1.8</option>  
      <!--默认开启，不做收缩（删除注释、未被引用代码）-->  
      <option>-dontshrink</option>  
      <!--默认是开启的，这里关闭字节码级别的优化-->  
      <option>-dontoptimize</option>  
      <!--  ##对于类成员的命名的混淆采取唯一策略-->  
      <option>-useuniqueclassmembernames</option>  
      <!--混淆类名之后，对使用Class.forName('className')之类的地方进行相应替代-->  
      <option>-adaptclassstrings</option>  
      <!--混淆时不生成大小写混合的类名，默认是可以大小写混合-->  
      <option>-dontusemixedcaseclassnames</option>  
      <!-- 忽略warn消息-->  
      <option>-ignorewarnings</option>  
      <option>-keep class org.apache.logging.log4j.util.* { *; }</option>  
      <option>-dontwarn org.apache.logging.log4j.util.**</option>  
      <!--对异常、注解信息在runtime予以保留，不然影响springboot启动-->  
      <option>-keepattributes Exceptions,InnerClasses,Signature,Deprecated,SourceFile,LineNumberTable,*Annotation*,EnclosingMethod</option>  
      <!--不混淆所有interface接口-->  
      <option>-keep interface * extends * { *; }</option>  
      <!--保留枚举成员及方法-->  
      <option>-keepclassmembers enum * { *; }</option>
      <!--保留枚举成员及方法-->  
      <option>-keepparameternames</option>  
      <!--保留main方法的类及其方法名-->  
      <option>-keepclasseswithmembers public class * { public static void main(java.lang.String[]);}</option>  
      <!--忽略note消息，如果提示javax.annotation有问题，那麽就加入以下代码-->  
      <option>-dontnote javax.annotation.**</option>  
      <option>-dontnote sun.applet.**</option>  
      <option>-dontnote sun.tools.jar.**</option>  
      <option>-dontnote org.apache.commons.logging.**</option>  
      <option>-dontnote javax.inject.**</option>  
      <option>-dontnote org.aopalliance.intercept.**</option>  
      <option>-dontnote org.aopalliance.aop.**</option>  
      <option>-dontnote org.apache.logging.log4j.**</option>  
      <!--  以下为需要根据项目情况修改 -->  
      <option>-keep class com.tophant.agent.config.* {*;}</option>  
      <option>-keep class com.tophant.agent.controller.* {*;}</option>  
      <option>-keep class com.tophant.agent.dto.* {*;}</option>  
      <option>-keepdirectories org.springframework.boot.autoconfigure</option>  
      <!--不混淆所有类,保存原始定义的注释-->  
      <option>-keepclassmembers class * { @org.springframework.beans.factory.annotation.Autowired *; @org.springframework.beans.factory.annotation.Qualifier *; @org.springframework.beans.factory.annotation.Value *; @org.springframework.beans.factory.annotation.Required *; @org.springframework.context.annotation.Configuration *; @org.springframework.stereotype.Service *; @org.springframework.context.annotation.Bean *; @org.springframework.context.annotation.Primary *; @org.springframework.boot.context.properties.ConfigurationProperties *; @org.springframework.boot.context.properties.EnableConfigurationProperties *; @javax.inject.Inject *; @javax.annotation.PostConstruct *; @javax.annotation.PreDestroy *; }</option> 
    </options> 
  </configuration>  
  <dependencies> 
    <dependency> 
      <groupId>net.sf.proguard</groupId>  
      <artifactId>proguard-base</artifactId>  
      <version>6.2.2</version> 
    </dependency> 
  </dependencies> 
</plugin>
```

## 问题

1. 因为`MyBatis`和`mapper`的`xml`需要进行映射，映射方式是以名称进行的，所以所以这部分代码不能进行混淆，有两种方法解决这一个问题
    
    1. `keep`代码中`dao`所在包的所有类
    2. `keep`代码中继承了`BaseMapperIntegration`的代码的类
2. 因为接口参数解析的问题，在使用了`POJO`作为数据承载的地方代码不能进行混淆，也就是`Controller`和所使用的`Entity`、`Model`均不能进行混淆
    
3. 配置类和动态注解的类不能进行混淆，不然在`Bean`装载的时候会直接提示类找不到
    
4. 启动的时候提示`Bean`重复，这是因为`Spring`代理在加载`bean`的时候名称生成策略的问题，修改生成方式就可以解决这一个问题，解决代码如下：
    
    ```java
    @SpringBootApplication
    public class AgentApplication {
    
        public static void main(String[] args) {
            ...
            } else {
                new SpringApplicationBuilder(AgentApplication.class)
                        .beanNameGenerator(new CustomGenerator())
                        .run(args);
            }
        }
    
    ...
    
        // 这里为bean名称生成策略所在位置
        public static class CustomGenerator implements BeanNameGenerator {
    
            @Override
            public String generateBeanName(BeanDefinition beanDefinition, BeanDefinitionRegistry beanDefinitionRegistry) {
                return beanDefinition.getBeanClassName();
            }
        }
    ```
    
5. `Spring`相关的`XML`里面，如果单独配置了`Bean`和`Bean`属性的，这类`bean`要保留，不能被混淆。使用`Spring Context`的`Bean`名称获取`Bean`的方式得换成依据jar的路径获取`Bean`
    

# 加密

## Jar包加密

### xjar工具

Spring Boot Jar 安全加密的运行工具，基于对jar包内资源的加密以及拓展`ClassLoader`来构建一套程序加密启动，动态解密运行的方案。

采用`xjar-maven-plugin`插件直接集成到代码中进行运行，相关pom.xml配置如下：

```xml
<!-- 指定插件仓库，这是为了能下载xjar的插件 -->
<pluginRepositories> 
  <pluginRepository> 
    <id>jitpack.io</id>  
    <url><https://jitpack.io></url> 
  </pluginRepository> 
</pluginRepositories>

<!-- xjar插件的相关配置 -->
<build> 
  <plugins> 
    <plugin> 
      <groupId>com.github.core-lib</groupId>  
      <artifactId>xjar-maven-plugin</artifactId>  
      <version>4.0.1</version>  
      <executions> 
        <execution> 
          <goals> 
            <goal>build</goal> 
          </goals>  
          <phase>package</phase>  
          <configuration> 
            <!-- 使用命令行代码生成最终的密码，这里只是一个占位免得报错 -->            
            <password>这是一个测试</password> 
          </configuration> 
        </execution> 
      </executions> 
    </plugin> 
  </plugins> 
</build>
```

以`client-api`为例，分支为`feature/xjar`，最终生成的`target`目录的下 文件结构如下：

```
.
├── classes
├── client-api.jar
├── client-api.jar.original
├── client-api.xjar
├── generated-sources
├── generated-test-sources
├── maven-archiver
├── maven-status
├── test-classes
└── xjar.go
```

这个时候需要关注的是`xjar.go`和`client-api.xjar`这两个文件，`xjar.go`文件可以需要在各个平台上进行编译（在`ArchLinux`上使用交叉编译`Windows`执行文件无法正常解密），`client-api.jar`是最终的运行文件，运行的时候执行如下命令就可以正常运行程序了：

```bash
./xjar java -jar client-api.xjar
```

一些问题：

1. 打包后需要独立的启动器才能正常的进行启动，但是会同时保留了未加密的`jar`包
2. 独立启动器是一对一的，每次都不同，并且涉及到通过命令启动的代码都得完全进行修改用来适应这一个改变

## 镜像加密

在使用镜像加密这一块，支持镜像加密的只是OCI的标准，核心是基于[ocicrypt](https://github.com/containers/ocicrypt)这个库。而Docker的moby项目在迁移到containerd上还不完善，对oci标准不能完全进行支持，比如镜像安全方面，所以由containerd生成的加密镜像在Docker上无法使用，会报解压错误。

下面使用的是不依赖K8S的情况下，直接使用containerd进行镜像的加密和解密直接运行的情况。这个的基本原理就是先将代码打包成镜像，然后对已有的镜像再进行加密，最后运行的时候使用的密钥直接运行镜像。

### 编译imgcrypt

首先我们得先编译[imgcrypt](https://github.com/containerd/imgcrypt)这个库，这个时候得要求系统上有Go的环境和基本的编译工具，执行的命令如下：

```bash
# 下载源码
<https://github.com/containerd/imgcrypt>

# 编译和安装
sudo make && sudo make install
```

### 启动containerd

创建config.toml，在其中填入如下内容：

```
disable_plugins = ["cri"]
root = "/tmp/var/lib/containerd"
state = "/tmp/run/containerd"
[grpc]
  address = "/tmp/run/containerd/mycontainerd.sock"
  uid = 0
  gid = 0
[stream_processors]
    [stream_processors."io.containerd.ocicrypt.decoder.v1.tar.gzip"]
        accepts = ["application/vnd.oci.image.layer.v1.tar+gzip+encrypted"]
        returns = "application/vnd.oci.image.layer.v1.tar+gzip"
        path = "/usr/local/bin/ctd-decoder"
    [stream_processors."io.containerd.ocicrypt.decoder.v1.tar"]
        accepts = ["application/vnd.oci.image.layer.v1.tar+encrypted"]
        returns = "application/vnd.oci.image.layer.v1.tar"
        path = "/usr/local/bin/ctd-decoder"
```

然后使用如下命令直接启动containerd

```bash
sudo containerd -c config.toml
```

在这里可能会提示cri加载不成功报错的问题，就算配置里面禁用了还是会提示这一个报错，这个时候直接创建如下的配置文件:

```bash
# 创建配置文件夹
mkdir -p /etc/cni/net.d

# 写入如下两个配置文件，这里只是测试，后期需要专门修改
cat >/etc/cni/net.d/10-mynet.conf <<EOF
{
	"cniVersion": "0.2.0",
	"name": "mynet",
	"type": "bridge",
	"bridge": "cni0",
	"isGateway": true,
	"ipMasq": true,
	"ipam": {
		"type": "host-local",
		"subnet": "10.22.0.0/16",
		"routes": [
			{ "dst": "0.0.0.0/0" }
		]
	}
}
EOF

cat >/etc/cni/net.d/99-loopback.conf <<EOF
{
	"cniVersion": "0.2.0",
	"name": "lo",
	"type": "loopback"
}
EOF
```

### 生成密钥

使用openssl生成公钥和私钥：

```bash
# 生成私钥
openssl genrsa -out mykey.pem

# 生成公钥
openssl rsa -in mykey.pem -pubout -out mypubkey.pem
```

### 镜像加密

使用如下的命令进行镜像加密：

```bash
sudo chmod 0666 /tmp/run/containerd/containerd.sock

# 获取镜像
sudo ctr-enc -a /tmp/run/containerd/containerd.sock images pull --all-platforms docker.io/library/bash:latest

# 加密镜像
sudo ctr-enc -a /tmp/run/containerd/containerd.sock images encrypt --recipient jwe:mypubkey.pem --platform linux/amd64 docker.io/library/bash:latest bash.enc:latest\\nEncrypting docker.io/library/bash:latest to bash.enc:latest
```

通过layerinfo可以看到加密后的镜像带上了加密的信息

![[assets/61c1951759c8d321c2224bbfb23809ad_MD5.jpeg]]

### 运行

可以将生成好的镜像上传到registry中，从中获取后直接进行运行：

```bash
sudo ctr-enc -a /tmp/run/containerd/containerd.sock run --rm --key mykey.pem localhost:5000/bash.enc:latest test echo 'Hello World!'
```

参考文档：

1. [https://docs.docker.com/engine/security/](https://docs.docker.com/engine/security/)
2. [容器安全——镜像加密](https://duyanghao.github.io/kubernetes_image_encryption/)
3. [https://github.com/moby/moby/pull/38738](https://github.com/moby/moby/pull/38738)
4. [https://github.com/containernetworking/cni](https://github.com/containernetworking/cni)
5. [Encrypting container images with skopeo](https://medium.com/@lumjjb/encrypting-container-images-with-skopeo-f733afb1aed4)