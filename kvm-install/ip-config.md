```
cat /etc/yum.repos.d/CentOS-Base.repo
```




```
yum -y install libguestfs-tools-c
```




```
virsh destroy template-v1

mkdir  /tmp/template-v1


```



```
cat >  /tmp/template-v1/ifcfg-eth0  << EOF
TYPE=Ethernet
BOOTPROTO=none
DEFROUTE=yes
NAME=eth0
DEVICE=eth0
ONBOOT=yes
IPADDR=10.200.100.102
PREFIX=24
GATEWAY=10.200.100.2
DNS1=223.5.5.5
EOF
```



```
virt-copy-in   -d     template-v1         /tmp/template-v1/ifcfg-eth0    /etc/sysconfig/network-scripts/
```















