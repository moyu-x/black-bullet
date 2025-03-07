# Kubernates

## Mac 开启 k8s

因为墙存在的原因，所以得变通下安装的方法，这个时候请参考[maguowei/k8s-docker-for-mac](https://github.com/maguowei/k8s-docker-for-mac)上的解决办法，通过使用国内的镜像源和预先下载好镜像，并且主要一定要使用`Edge`版本的`Docker`

## Helm 的安装

`Helm`大部分的安装都可以按照文档进行处理，但是提示`tiller`需要升级，并且发现`titler`不能启动，这个时候会提示拉不到这个镜像，解决办法是先查看镜像的版本是多少：

```
kubectl describe pod tiller-deploy-668c95696c-jtnb9 -n kube-system
```

然后使用[azure](tips_[http:_mirror.azure.cn_help_gcr-proxy-cache.html](http://_mirror.azure.cn_help_gcr-proxy-cache.html/)的镜像将其获取下来，最后修改`deploment`中的镜像数据：

```bash
# 获取对应版本的 Tiller
docker pull gcr.azk8s.cn/kubernetes-helm/tiller:v2.14.0
# 获取当前 Tiller 的 deployment 然后修改其中的数据
kubectl get deployment tiller-deploy -n kube-system -o yaml > tiller-deploy.yml
# 删除原有的 deployment
kubectl delete deployment tiller-deploy -n kube-system
# 使用修改后的 deployment
kubectl apply -f tiller-deploy.yml
```

## Windows 安装

在`Windows`上安装`kubernetes`的安装的主体流程和在`Mac`上安装差不多，但是遇到的问题也是措手不及的，首先需要将配置中的`DNS`又原来的`automatic`改成`fix`并建议设置成`114.114.114.114`，然后在系统防火墙中的出入站规则中加入一个出站规则，入站的两个规则已经在安装的时候创建成功了，加入的是`vpnkit`这个程序的规则。

最主要的一点是，安装中出现问题，可以到`C:\\Program Data\\DockerDesktop\\service.txt`中查看相关报错并解决。