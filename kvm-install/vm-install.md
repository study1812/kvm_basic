注意 如果是使用yum 需要再次mount 

```
mount /dev/cdrom  /mnt/
```


##### 上传ISO 镜像文件到指定目录


使用 Xftp  上传ISO
```
cd /kvm/iso/

```




```
ls /kvm/iso
```


##### 启动virt-manager  从 iso 安装第一个虚拟机 


```
选择 网络 br0 virtIO 
磁盘 qcow2 virtIO  大小3G
显示 vnc   ens-us
开始安装
```

安装操作系统 注意

```
使用 标准分区 文件系统 xfs 全部给根分区  /

标准分区 
/ 3G  xfs
没有  swap
没有  boot
```

虚拟机名字

```
CentOS-7-x86_64-1804
```

虚拟磁盘名字

```
CentOS-7-x86_64-1804.qcow2
```


关闭虚拟机 备份 虚拟磁盘

```
cp /kvm/image/CentOS-7-x86_64-1804.qcow2   /kvm/image/CentOS-7-x86_64-1804.qcow2-bak
```

备份虚拟机 xml  文件

```
cp /etc/libvirt/qemu/CentOS-7-x86_64-1804.xml  /etc/libvirt/qemu/CentOS-7-x86_64-1804.xml-bak
```
