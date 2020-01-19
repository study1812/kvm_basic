```
#!/bin/bash
images_dir=/kvm/image
xml_dir=/etc/libvirt/qemu

read -p "请输入新建虚拟机名称: " new_kvm

if [  -f "${xml_dir}/${new_kvm}.xml" ];then
		echo "${new_kvm} 已存在,部署程序退出..."
        	exit 1
fi

if [  -f "${images_dir}/${new_kvm}.qcow2" ];then
                echo "${new_kvm}.qcow2  已存在,部署程序退出..."
                exit 1
fi

virsh vol-clone  CentOS-7-x86_64-1804.qcow2  --pool kvmimage ${new_kvm}.qcow2


read -p "请输入新建虚拟机磁盘大小单位G: " disk_size


echo "开始创建虚拟磁盘"
virsh  vol-resize  ${new_kvm}.qcow2  --pool kvmimage  --capacity $disk_size'G'


read -p "请输入新建虚拟机内存大小单位M   : " mem_size

read -p "请输入新建虚拟机cpu核心数 : " core_num







################配置虚拟机模板#######################################################################################################################################################################################
cp ${xml_dir}/template.xml   ${xml_dir}/${new_kvm}.xml
sed  -i  "s#.*:.*:.*:.*:.*:.*#<mac address='`MACADDR="52:54:$(dd if=/dev/urandom count=1 2>/dev/null | md5sum | sed -r 's/^(..)(..)(..)(..).*$/\1:\2:\3:\4/')"; echo $MACADDR`'/>#g"   ${xml_dir}/${new_kvm}.xml
sed    -i  "s#<uuid>.*-.*-.*-.*-.*</uuid>#<uuid>`uuidgen`</uuid>#g"      ${xml_dir}/${new_kvm}.xml 
sed -i     "s#template#${new_kvm}#g"           ${xml_dir}/${new_kvm}.xml
sed    -i  "s#mem-size#$mem_size#g"      ${xml_dir}/${new_kvm}.xml
sed    -i  "s#core-num#$core_num#g"      ${xml_dir}/${new_kvm}.xml
#######################################################################################################################################################################################################################



virsh define   ${xml_dir}/${new_kvm}.xml  


virsh autostart ${new_kvm}

                echo "创建成功"



read -p "请输入为虚拟机配置的ip: " new_kvm_ip

mkdir /tmp/${new_kvm}

echo "NAME=eth0"   >               /tmp/${new_kvm}/ifcfg-eth0
echo "DEVICE=eth0" >>              /tmp/${new_kvm}/ifcfg-eth0
echo "ONBOOT=yes" >>               /tmp/${new_kvm}/ifcfg-eth0
echo "TYPE=Ethernet" >>    /tmp/${new_kvm}/ifcfg-eth0
echo "BOOTPROTO=none" >>   /tmp/${new_kvm}/ifcfg-eth0
echo "IPADDR=${new_kvm_ip}"   >>    /tmp/${new_kvm}/ifcfg-eth0
echo "NETMASK=255.255.255.0"  >>   /tmp/${new_kvm}/ifcfg-eth0
echo "GATEWAY=10.200.100.2"   >>   /tmp/${new_kvm}/ifcfg-eth0
echo "DNS1=223.5.5.5"   >>   /tmp/${new_kvm}/ifcfg-eth0

cat  /tmp/${new_kvm}/ifcfg-eth0

read -p "请确认您的网卡配置，按y键继续?[y/n]: " eth0_config
	if [ ! "${eth0_config}" = "y" ];then
		echo "输入错误!"
        	exit 1
	fi


echo "开始配置$new_kvm 的网卡，请稍后......."

virt-copy-in   -d     $new_kvm        /tmp/${new_kvm}/ifcfg-eth0    /etc/sysconfig/network-scripts/

virsh start  $new_kvm   1>/dev/null

echo "正在启动虚拟机请稍后....."

sleep 15s

echo "虚拟机启动成功，登录赋值如下命令即可  ssh ${new_kvm_ip}"

rm -rf /tmp/${new_kvm}  2>/dev/null
```
