
##### console 设置  


使之可以使用 virsh console 命令登陆虚拟机

```
grubby --update-kernel=ALL --args="console=ttyS0"
reboot
```


```
virsh console xxx
```



#### 模板虚拟机需要联网安装软件包  配置ip

```
cat > /etc/sysconfig/network-scripts/ifcfg-eth0   << EOF
TYPE=Ethernet
BOOTPROTO=none
DEFROUTE=yes
NAME=eth0
DEVICE=eth0
ONBOOT=yes
IPADDR=10.200.100.101
PREFIX=24
GATEWAY=10.200.100.2
DNS1=223.5.5.5
EOF
```


```
systemc restart network
```


退出console 使用 ssh  连接配置的ip 地址

```
退出console
ctrl+]
```





####  配置模板虚拟机 使之接收宿主机的 virsh shutdown 命令

kvm 虚拟机安装
```
yum install -y acpid
systemctl enable acpid
```


#####  安装扩展虚拟磁盘  需要使用的软件包

```
yum install cloud-utils-growpart -y
```


#### 使用virt-manager  安装qga

> https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/virtualization_deployment_and_administration_guide/chap-qemu_guest_agent


模板虚拟机内部安装 qga

```
yum -y install qemu-guest-agent
```

```
systemctl start qemu-guest-agent
```



