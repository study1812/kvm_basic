查看并设置字符集


```
export LANG="en_US.UTF-8"
```

#### 设置yum 



##### 备份 repo
```
mkdir  /etc/yum.repos.d/bak
mv /etc/yum.repos.d/*.repo  /etc/yum.repos.d/bak/
```



##### 挂载ISO

```
mount /dev/cdrom  /mnt/
```

##### 创建repo

```
cat > /etc/yum.repos.d/base.repo << EOF
[base]
name=base
baseurl=file:///mnt
gpgcheck=0
EOF
```

##### 测试repo
```
yum clean all
yum -y install vim
```


#### 在系统上创建网桥设备, 名字为br0 并将eth0 桥接到br0

```
yum -y install bridge-utils
```



```
cat >  /etc/sysconfig/network-scripts/ifcfg-br0  << EOF
DEVICE=br0
ONBOOT=yes
BOOTPROTO=static
NM_CONTROLLED=no
IPADDR=10.200.100.11
NETMASK=255.255.255.0
GATEWAY=10.200.100.2
DNS1=223.5.5.5
USERCTL=no
TYPE=Bridge
EOF
```



```
cat >  /etc/sysconfig/network-scripts/ifcfg-eth0  << EOF
DEVICE=eth0
ONBOOT=yes
BRIDGE=br0
EOF
```

```
systemctl  restart network
```

######  重启网络后 ssh 连接 新设置的ip  10.200.100.11



#### 检查宿主机是否支持  kvm

```
egrep '(vmx|svm)' /proc/cpuinfo
```

#### 安装kvm 


```
yum   -y   install   libvirt*  
```

```
systemctl  start libvirtd
```

#### 创建 一个kvm 网络  类型  bridge 名字 br0

##### 创建一个kvm 网络的 xml 文件 

```
echo "<network><name>br0</name><uuid>`uuidgen`</uuid><forward mode='bridge'/><bridge name='br0'/></network>"  > /etc/libvirt/qemu/networks/br0.xml
```

##### 从xml文件创建一个kvm网络
```
virsh net-define    /etc/libvirt/qemu/networks/br0.xml
```

##### 启动kvm 网络br0

```
virsh net-start   br0
```
##### 设置br0 随着 libvirtd 启动而一起启动

```
virsh  net-autostart  br0
```


#### 创建 两个 kvm pool  类型为 dir 

##### 创建 kvm pool 自定义目录

```
mkdir -p /kvm/image
mkdir  -p /kvm/iso
```

##### 创建 两个 kvm pool   名字是 kvmimage ISO
```
virsh pool-define-as  kvmimage  dir --target  "/kvm/image"
virsh pool-define-as  iso  dir --target  "/kvm/iso"
```


##### 启动 kvm  pool
```
virsh pool-start  kvmimage
virsh pool-start  iso
```


##### 设置 kvm pool 随 libvirtd 启动而启动
```
virsh  pool-autostart   kvmimage
virsh  pool-autostart   iso
```

#### 安装virt-manager  图形工具

```
yum   -y   install    virt-manager xorg*
```

重新ssh连接 10.200.100.11  直到不再出现如下提示

```
Last login: Tue Jun 25 22:19:59 2019 from 192.168.1.100
/usr/bin/xauth:  file /root/.Xauthority does not exist
```


在xshell 中再次连接宿主机  可以多尝试几次 只到 没有再出现 之类的提示 

输入 virt-manager 命令  即可打开图形工具

```
[root@localhost ~]# virt-manager 
[root@localhost ~]# libGL error: unable to load driver: swrast_dri.so
libGL error: failed to load driver: swrast

```


```
yum -y install mesa-dri-drivers
```
