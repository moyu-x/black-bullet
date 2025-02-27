# 磁盘不可用

提示`TASK ERROR: activating LV 'st-disk/st-disk' failed: Activation of logical volume st-disk/st-disk is prohibited while logical volume st-disk/st-disk_tmeta is active.`

解决方案如下

```bash
lvchange -an pve/data_tmeta  
lvchange -an pve/data_tdata

lvchange -ay pve
```

ref:
1. [proxomx 虚机无法启动的奇怪问题-CSDN 博客](https://blog.csdn.net/feitianyul/article/details/125417765)
2. [PVE 提示禁止激活逻辑卷的解决方法 | 一名技术渣的博客](https://blog.rootwhois.cn/archives/pve-ti-shi-jin-zhi-ji-huo-luo-ji-juan-de-jie-jue-fang-fa.html)

# 相关资源

- [xiangfeidexiaohuo/ProxmoxVE-7.0-DIY: Proxmox VE 7.x 换源、关掉订阅提示、直通、配置 CPU 模式、升降内核等相关教程。](https://github.com/xiangfeidexiaohuo/ProxmoxVE-7.0-DIY)

