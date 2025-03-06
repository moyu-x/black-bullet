## 原因

1. 开发过程很容易遇到需要导出数据进行分析的情况
2. 需要对导出的数据进行处理，如果不处理直接找一个RDMS软件进行处理就行
3. 使用Python+Pandas结合起来方便，代码量少，还能对导出的数据进行处理

## 环境依赖

以下是`Pipfile`中的文件

```
records = "*"
mysqlclient = "*"
pandas = "*"
sqlalchemy = "*"
pymongo = "*"
openpyxl = "*"
```

## 代码主体逻辑

1. 数据库连接
2. 需要执行的 SQL
3. 导出 Excel 或者迁移到其他数据库

## 代码示例

下面这段代码因为就一次使用，就没封装函数那些：

```python
import pandas
from sqlalchemy import create_engine

client_db = create_engine(
    'mysql://username:password@host:port/database?charset=utf8mb4')

# TODO 在这写入合适的SQL
fr_sql = ''''''

fr_df = pandas.read_sql(fr_sql, con=client_db)

# TODO 对导出数据的 DataFrame 进行处理和修改
for index, row in fr_df.iterrows():
    # 处理代码
    fr_df.to_excel('example.xlsx', index=False)
```