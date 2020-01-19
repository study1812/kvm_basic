勘误：


```
口误:
vitio 和写时复制技术没有必然联系
qcow2 是一种基于写时复制技术的磁盘格式它支持virtio 类型的驱动
```

libvirtd 中主要的对象



|主要对象|  解释|
|:----|:------|
|node   |  宿主机|
|domain | 虚拟机|
|net     |虚拟网络|
|pool   | 存储池|
|vol  | 存储卷|






```
https://www.libvirt.org/virshcmdref.html
```


```
https://libvirt.org/sources/virshcmdref/Virsh_Command_Reference-0.8.7-1.pdf
```

```
wget https://libvirt.org/sources/virshcmdref/Virsh_Command_Reference-0.8.7-1.pdf
```



##### virsh 命令自动补全

使用互联网的yum仓库

```
mv  /etc/yum.repos.d/base.repo    /etc/yum.repos.d/base.repo-bak
cp /etc/yum.repos.d/bak/CentOS-Base.repo  /etc/yum.repos.d/
```


```
yum install -y bash-completion libvirt-bash-completion
yum -y install libvirt-client
source  /usr/share/bash-completion/completions/vsh
source /etc/profile
```




