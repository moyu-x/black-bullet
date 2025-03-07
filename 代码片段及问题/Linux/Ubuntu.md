# 清理多余内核

查看内核信息：

```bash
dpkg --get-selections | grep linux
```

状态为deinstall即已经卸载，如果觉得看着不舒服的话可以使用purge连配置文件里一起彻底删除，清理内核列表。删除多余的内核，如下：

```bash
sudo apt autoremove linux-headers-5.4.0-52 linux-headers-5.4.0-52-generic linux-image-5.4.0-52-generic linux-modules-5.4.0-52-generic linux-modules-extra-5.4.0-52-generic linux-modules-nvidia-450-5.4.0-52-generic
```

或

```bash
sudo apt --purge autoremove linux-headers-5.4.0-52 linux-headers-5.4.0-52-generic linux-image-5.4.0-52-generic linux-modules-5.4.0-52-generic linux-modules-extra-5.4.0-52-generic linux-modules-nvidia-450-5.4.0-52-generic
```

删除内核后需要更新grub移除失效的启动项：

```bash
sudo update-grub
```

# 修改DNS

1. 编辑`/etc/systemd/resolved.conf`
2. 修改其中的DNS字段
3. 然后`systemctl restart systemd-resolved.service`

## WSL2 升级

当遇到`Command terminated with exit status 1`的时候，先执行`sudo apt remove snpad`，然后执行升级就可以了