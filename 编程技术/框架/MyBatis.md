
## 面试题

### `${}`和`#{}`的区别

`${}`是Properties文件中变量占位符，用于表示标签属性值和sql内部，属于静态文本替换。

`#{}`是SQL参数展位符号，MyBatis会将XML中SQL里的`#{}`替换为`?`，在SQL执行前会使用`PreparedStatement`的参数设置方法，将`?`替换为实际的参数值

### MyBatis的分页和原理

MyBatis使用RowBounds对象进行分页，它是正对ResultSet结果执行的内存分页，而非物理分页，可以在SQL内直接书写带有物理分页的参数来完成物理分页，也可以使用分页插件来完成物理分页。

分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的SQL，然后重写SQL，根据数据库的dialect方言，添加对应的物理分页语句和分页参数。

### 分页插件的原理

MyBatis仅可以编写针对`ParameterHandler`、`ResultSetHandler`、`StatementHandler`及`Executor`这四个接口的插件，MyBatis使用JDK的动态代理，为需要拦截的接口生成代理对象以实现接口方法拦截功能，每当执行者四个接口对象的方法时，就会进入拦截方法。

### 动态SQL及其原理

使用OGNL从SQL参数对象中计算表达式的值，根据表达式的值动态拼接SQL，以此来完成SQL的功能

### 延迟加载

MyBatis仅支持association关联对象和collection关联集合对象的延迟加载。在MyBatis的配置中，可以使用`lazyLoadingEnableed=true`来启用延迟加载