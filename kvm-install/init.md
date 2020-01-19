##### 系统初始化设置


###### 关闭selinux

```
sed -i 's/enforcing/disabled/g'  /etc/selinux/config
setenforce 0
```


###### 关闭sshd 的 UseDNS 参数

```
sed -i 's/#UseDNS yes/UseDNS no/g'   /etc/ssh/sshd_config
systemctl   restart sshd
```



###### 关闭 NetworkManager 和 firewalld

```
systemctl  disable firewalld NetworkManager
systemctl  stop  firewalld  NetworkManager
```


###### 设置固定ip 并重启网络

```
rm -rf  /etc/sysconfig/network-scripts/ifcfg-eth0

cat > /etc/sysconfig/network-scripts/ifcfg-eth0   << EOF 
TYPE=Ethernet
BOOTPROTO=none
NAME=eth0
DEVICE=eth0
ONBOOT=yes
IPADDR=10.200.100.11
NETMASK=255.255.255.0
GATEWAY=10.200.100.2
DNS1=223.5.5.5
EOF
```


```
systemctl  restart network
```

###### 关闭虚拟机  在vmware 中克隆 

###### 取消 VMnet8  网络的dhcp
