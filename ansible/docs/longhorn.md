# Install ISCSI

```bash
# Install in master and workers
sudo apt install open-iscsi util-linux nfs-common cryptsetup -y
sudo systemctl enable --now iscsid
```



