# 环境变量的问题

一定要将当前目录添加到`GOPATH`中，不然在代码补全或者使用`dep`管理依赖的时候会提示当前目录不再`GOPATH`中

# Dep 管理依赖

当使用`dep`初始化一个空目录的时候，也就是没有`Go`代码的情况下，会提示报错，这个时候最简单的解决方法就是在当前目录下面创建一个空的`Go`代码，这个时候就不会有报错的提示了

```
touch main.go
```

# VSCode 提示扩展安装失败

在`VSCode`中安装`Go`的扩展后，会提示安装一些以来组建，这可以参考[官方的 WIKI](https://github.com/Microsoft/vscode-go/wiki/Go-tools-that-the-Go-extension-depends-on)来进行安装。在做完这些后，有可能会提示`dlv`和`gocode-mod`安装失败，这个时候`dlv`可以直接在发行版中找到(我用的是 ArchLinux)，但是`gocode-mod`安装失败主要是名称问题，这个时候，执行如下命令就可以的，虽然这操作有点迷：

```bash
go get -u -v github.com/ianthehat/godefgo get -u -v github.com/stamblerre/gocode# and in $GOPATH/bin use the first folder of the gopath, if there are several.....cp godef godef-gomodcp gocode gocode-gomod
```

# Array 循环修改值的问题

在`Go`语言中，`array`和`struct`都是值类型的，而`slice`、`map`、`chan`是引用类型，所以修改值会出现是把，这个最好的建议是使用`slice`替换`array`，如果是需要使用`array`并且修改值，这个时候需要进行如下操作：

```go
goArray := []GoArray{}
// 修改值的方式
for index := range goArray {    
    goArray[index].fieldA = value
}
```