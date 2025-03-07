# 使用问题

## 提示`Missing Pygments output`

在命令行中使用时候，加入如下参数`--shell-escape`

## texlive 2020 的包升级

```bash
# 设置为tuna源
tlmgr option repository <https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet>

# 查看更新包的列表
tlmgr update --list

# 更新所有可以更新的包
tlmgr update --self --all
```

## **指令**

### **更新 Texlive 指令**

```bash
**tlmgr update -all**
```

# 实用工具

- [表格生成](https://www.tablesgenerator.com/)
- [在线Latex公式编辑器](https://www.latexlive.com/)