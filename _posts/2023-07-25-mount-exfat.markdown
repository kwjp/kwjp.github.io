## 查看U盘位置
fdisk -l

## 挂载U盘
```Java
mount.exfat-fuse /dev/sda1 /samba/DLNA/Video/mnt/
mount.exfat-fuse /dev/sda2 /mnt/private_disk/
```

## 卸载U盘
umount /dev/sda1
