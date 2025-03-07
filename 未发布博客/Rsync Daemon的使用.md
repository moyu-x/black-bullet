1. 在Master节点修改`/etc/rsyncd.conf`，没有则创建此文件，需要注意：
    
    1. `hosts allow`用空格分割，写入允许的ip或者允许的IP段
    2. `hosts deny`建议直接设置`*`
    3. `port`字段修改为实际申请端口
    4. `[101to121]`为同步模块的名称，根据实际情况进行修改
    5. `auth users`为允许的用户名称，用空格进行分割
    6. `/data/rsync-temp`为需要同步文件的目录，根据实际情况进行修改
    
    ```bash
    uid = nobody
    gid = nobody
    port = 1025
    use chroot = yes
    hosts allow = 10.0.81.121 10.0.81.101
    hosts deny = *
    max connections = 4
    # pid file = /var/run/rsyncd.pid
    # exclude = lost+found/
    transfer logging = yes
    timeout = 900
    # ignore nonreadable = yes
    dont compress   = *.gz *.tgz *.zip *.z *.Z *.rpm *.deb *.bz2
    secrets file = /etc/rsync/rsyncd.secrets
    
    [101to121]
    path = /data/rsync-temp
    list = no
    ignore errors
    comment = tophant acl rsync
    auth users = tophant
    ```
    
2. 根据如下步骤创建`rsyncd.secrets`文件并编辑
    
    1. `mkdir -p /etc/rsync`
    2. `vim /etc/rsync/rsyncd.secrets`
    3. 写入`username:password`，例如`tophant:tophant`
    4. `chmod 600 /etc/rsync/rsyncd.secrets`
3. 执行`rsync --daemon --config=/etc/rsyncd.conf`
    
4. 在需要分析机器上创建`rsyncd.secrets`文件
    
    1. `mkdir -p /etc/rsync`
    2. `vim /etc/rsync/rsyncd.secrets`
    3. 写入`username:password`，例如`tophant:tophant`
    4. `chmod 600 /etc/rsync/rsyncd.secrets`
5. 执行如下命令进行`rsync -avzP --password-file=/etc/rsync/rsyncd.secrets --port 1025 --delete tophant@10.0.81.101::101to121 /root/temp`