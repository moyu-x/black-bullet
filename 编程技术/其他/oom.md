# 查看 oom 信息的方法

- 运行 `grep "Out of memory" /var/log/messages` 命令来查看系统日志。
- 运行 `egrep -i -r 'killed process' /var/log` 命令来查看系统日志。
- 运行 `dmesg` 命令来查看系统日志。
