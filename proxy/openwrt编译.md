# 自己编译openwrt版本


1. 官方openwrt
2. immortalwrt

## 网页版本构建 

官方openwrt构建：
[https://firmware-selector.openwrt.org](https://firmware-selector.openwrt.org/)

ImmortalWrt构建:
[https://firmware-selector.immortalwrt.org](https://firmware-selector.immortalwrt.org/)

[v2raya](https://mirror-03.infra.openwrt.org/releases/packages-23.05/x86_64/packages/v2raya_2.2.5.7-1_x86_64.ipk)


软件包名称查询：  
[https://mirror-03.infra.openwrt.org/releases/](https://mirror-03.infra.openwrt.org/releases/)  
软件包功能查询：  
[https://openwrt.org/packages/table/start](https://openwrt.org/packages/table/start)

Image Builder构建脚本下载：  
[https://github.com/qichiyuhub/OpenWrt-Builder](https://github.com/qichiyuhub/OpenWrt-Builder)  
根据自己需求编辑脚本，建议只添加必要的，其他的安装后软件商店安装即可。

## OpenWrt-Docker Image Builder

确保网络科学，并开启全局！  
确保docker空间充足，否则可能空间不足无法构建（至少1G以上）

创建普通用户：

```bash
	opkg update                                            # 更新软件包列表
	opkg install sudo shadow-useradd                       # 安装必要软件
	useradd -m -s /bin/ash qcy                             # 创建用户
	passwd qcy                                             # 设置密码
	echo "qcy ALL=(ALL) ALL" >> /etc/sudoers               # 添加sudo权限
```

退出root 用普通用户登录（su命令在这不生效，直接退出重进）

编辑好构建脚本  
构建脚本放入用户目录home/name下

用户目录递归777权限

进入脚本目录开始构建

cd /home/…..

sudo ./docker-build.sh

输入普通用户密码

## Linux-Docker Image Builder

安装好Linux，登录root账号、网络科学环境

安装sudo、curl、docker

```bash
	apt update
	sudo apt install sudo curl
	curl -fsSL https://get.docker.com | bash -s docker
```

普通用户给sudo权限（name修改为实际用户名）

bash

```bash
echo "name ALL=(ALL) ALL" | sudo EDITOR='tee -a' visudo
```

切换到普通用户（name修改为实际用户名）


```bash
su name
```


编辑好构建脚本  
构建脚本放入用户目录home/name下

用户目录递归777权限

进入脚本目录开始构建

cd /home/…..

sudo ./docker-build.sh

输入普通用户密码


```
config interface 'loopback'
        option device 'lo'
        option proto 'static'
        option ipaddr '127.0.0.1'
        option netmask '255.0.0.0'

config globals 'globals'
        option ula_prefix 'fd99:b7fe:f39b::/48'
        option packet_steering '1'

config device
        option name 'br-lan'
        option type 'bridge'
        list ports 'eth0'

config interface 'lan'
        option device 'br-lan'
        option proto 'static'
        option ipaddr '192.168.0.178'
        option netmask '255.255.255.0'
        option ip6assign '60'

config interface 'wan'
        option device 'eth1'
        option proto 'dhcp'

config interface 'wan6'
        option device 'eth1'
        option proto 'dhcpv6'
```

![[Screenshot 2024-11-09 at 21.38.59.png]]



安装argo主题包，完成后刷新就能变回熟悉的感觉

![[Pasted image 20241109214031.png]]


安装网页版terminal  ttyd

![[Pasted image 20241109214301.png]]

R5S 备份 network

TF卡启动直接使用[balenaEtcher](https://github.com/balena-io/etcher/releases)等工具写入OpenWrt固件至TF卡中即可, 如果EMMC中已有系统, 则设备会优先从EMMC中启动, 要改为默认从TF卡启动, 请从EMMC启动后使用以下命令清除eMMC上的 Loader

`dd if=/dev/zero of=/dev/mmcblk1p1 bs=8M count=25   dd if=/dev/zero of=/dev/mmcblk2 bs=8M count=25`


```bash
root@OpenWrt:~# cat /etc/config/network

config interface 'loopback'
	option ifname 'lo'
	option proto 'static'
	option ipaddr '127.0.0.1'
	option netmask '255.0.0.0'

config globals 'globals'
	option packet_steering '0'
	option ula_prefix 'fd07:0ca4:d7f6::/48'

config interface 'lan'
	option type 'bridge'
	option ifname 'eth1 eth2'
	option proto 'static'
	option netmask '255.255.255.0'
	option ip6assign '60'
	option ipaddr '192.168.0.201'
	option gateway '192.168.0.1'
	option dns '192.168.0.1'

config interface 'wan'
	option ifname 'eth0'
	option proto 'dhcp'

config interface 'wan6'
	option ifname 'eth0'
	option proto 'dhcpv6'

config interface 'vpn0'
	option ifname 'tun0'
	option proto 'none'

config interface 'ipsec_server'
	option ifname 'ipsec0'
	option device 'ipsec0'
	option proto 'static'
	option ipaddr '192.168.100.1'
	option netmask '255.255.255.0

```