## 不显示输入的数据

```bash
echo -n "请输入数据库密码："
read -s passwd
```

## 判断文件是否存在

```bash
# 先判断导出文件夹是否存在
if [ ! -d "./example" ]
then
  mkdir -p ./example
fi
```

## 条件语句

在判断的`expression`中，需要用`[]`将语句括起来

## 参数读取

### 变量赋值

在变量赋值的是一定要注意=号两端不能有任何的空格

![[assets/c45db22001bdfa024027f0d66ac626e9_MD5.jpeg]]
