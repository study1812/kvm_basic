```
chmod +x /etc/rc.d/rc.local

growpart   /dev/vda 1 && partprobe  && xfs_growfs /dev/vda1
```




再次扩盘不想关机重启


宿主机
```
virsh blockresize --path  /kvm/image/template-v1.qcow2    --size 15G   template-v1
```

虚拟机

```
fdisk -l
df -lh
growpart   /dev/vda 1 && partprobe  && xfs_growfs /dev/vda1
df -lh
```
