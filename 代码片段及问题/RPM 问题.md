# key 相关

## 导入 key

```bash
rpm --import XXX-RELEASE-GPG-KEY
```

## 删除 key

```bash
rpm -e gpg-pubkey-759d8517-5a5068a3
```

## 忽略 gpg key

```bash
rpm --nogpgcheck  xxx.rpm
```