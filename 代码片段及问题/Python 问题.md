# 指令

## Tuna 镜像

```bash
pip config set global.index-url <https://pypi.tuna.tsinghua.edu.cn/simple>
```

## 简单的 HTTP 服务

```bash
python3 http.server
```

## 获取目前已经安装的包

参考：[https://stackoverflow.com/questions/44210656/how-to-check-if-a-module-is-installed-in-python-and-if-not-install-it-within-t](https://stackoverflow.com/questions/44210656/how-to-check-if-a-module-is-installed-in-python-and-if-not-install-it-within-t)

```python
import pkg_resources

installed = {pkg.key for pkg in pkg_resources.working_set}
```

## `No module named 'win32com'`问题

这个是因为`pywin32`不存在引起的

```bash
pip install pywin32
```

# 库收集

[loguru](https://github.com/Delgan/loguru)：一个格式化日志输出工具

# Conda

## 环境创建

创建独立的可以包含`pip`的环境

```bash
conda create --name nCoV python=3.7 pip
```

## 创建虚拟环境

```bash
conda create -n name python=3.6
```

## 激活环境

```bash
conda activate env-name
```

## 反激活环境

```bash
conda deactivate
```

## 更新所有环境

```bash
conda update --all
```

## 清理安装包

```bash
conda clean --all
```

## Lint 工具

### Flake8 忽略错误

```python
except:  # noqa: E722
```

## Flask

### AssertionError: View function mapping is overwriting an existing endpoint function: batch_disposal

出现这种情况的原因是在视图层定义的时候出现了两个相同名称的视图定义，修改名称不重复后就可以使用了

## Pipenv

### 生成requirements文件

```python
pipenv lock -r > requirements.txt
```