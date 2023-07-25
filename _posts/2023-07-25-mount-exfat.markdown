## 查看U盘位置
fdisk -l

## 挂载U盘
mount.exfat-fuse /dev/sda1 /mnt/shared_disk/
mount.exfat-fuse /dev/sda2 /mnt/private_disk/

## 卸载U盘
umount /dev/sda1
