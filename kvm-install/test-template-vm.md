```
virsh vol-list --pool kvmimage
```

```
virsh vol-clone  CentOS-7-x86_64-1804.qcow2  --pool kvmimage template-v1.qcow2
```


```
virsh  vol-info template-v1.qcow2 --pool kvmimage
```


```
Name:           template-v1.qcow2
Type:           file
Capacity:       3.00 GiB
Allocation:     1.50 GiB
```


```
virsh  vol-resize  template-v1.qcow2 --pool kvmimage  --capacity 10G
virsh  vol-info template-v1.qcow2 --pool kvmimage
```


```
Name:           template-v1.qcow2
Type:           file
Capacity:       10.00 GiB
Allocation:     1.50 GiB
```


###  通过 virt-manager  使用存在的 image 创建虚拟机


测试虚拟机名字 template-v1  




###  测试  virsh shutdown 


```
virsh shutdown
```

### 测试扩展虚拟机文件系统


```
df -lh
LANG=en_US.UTF-8
growpart   /dev/vda 1
xfs_growfs /dev/vda1
df -lh
```



###  测试 qga



```
virsh  set-user-password   template-v1   --user root --password 654321
```









