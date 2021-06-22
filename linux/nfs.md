```bash
yum -y install nfs-utils
vim /etc/exports
/mnt/nfs-data *(rw,sync,no_root_squash)
systemctl enable rpcbind
systemctl enable nfs-server --now
showmount -e
```

