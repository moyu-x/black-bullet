# 空口令

执行`select user,host ,Password from mysql.user;`，会看到如下一些空值的情况

![[assets/8046b4e406de317b38dac99f67742bb8_MD5.jpeg]]

执行如下命令来清除和修改密码：

```mysql
# 删除空用户
delete from mysql.user where user='';

# 更新密码
set password for '用户名'@localhost = password('密码');

# 刷新权限
flush privileges;
```
