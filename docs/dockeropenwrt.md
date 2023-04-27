# 使用Docker安装Openwrt软路由

系统Debian11，没什么其它原因，，VPS上一直用的这个。官网下个镜像，烧U盘上，再随便找个教程照着装就行了，英文可以的不用教程，全程基本傻瓜操作，跟着导航走就行。只装个命令行就行，不用图形界面，浪费性能资源也没什么用。

装完配好网络，能上网后装上Openssh server 开启访问权限，以后就可以通过任意设备SSH远程登陆操作小主机了。


## 安装Docker

Docker可以使用官方的安装脚本：

```sh
curl -sSL https://get.docker.com/ | sh
```

据说因为国内网络的限制可能会出错，但我自己直接用的没什么问题。出错的可以使用国内的镜像脚本，两个随意一个：

阿里云脚本

```sh
curl -sSL http://acs-public-mirror.oss-cn-hangzhou.aliyuncs.com/docker-engine/internet | sh -
```

DaoCloud 的安装脚本

```sh
curl -sSL https://get.daocloud.io/docker | sh
```


## 安装Docker compose（选装）

compose可以根据个人需要，用到的时候再装就可以。

Linux 上我们可以从 Github 上下载它的二进制包来使用，最新发行的版本地址：https://github.com/docker/compose/releases。

运行以下命令以下载 Docker Compose 的当前稳定版本：


```sh
$ sudo curl -L "https://github.com/docker/compose/releases/download/v2.2.2/docker-compose-$(uname -s)-$(uname -m)"  -o /usr/local/bin/docker-compose
```


要安装其他版本的 Compose，请替换 v2.2.2。

Docker Compose 存放在 GitHub，不太稳定。你可以也通过执行下面的命令，高速安装 Docker Compose。

curl -L https://get.daocloud.io/docker/compose/releases/download/v2.4.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

将可执行权限应用于二进制文件：

```sh
$ sudo chmod +x /usr/local/bin/docker-compose
```

创建软链：

```sh
$ sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

测试是否安装成功：

```sh
$ docker-compose version

cker-compose version 1.24.1, build 4667896b
```


## Openwrt安装

Openwrt的镜像选择还是比较多的，在Docker hub以此为关键字可以检索出很多，根据自己需要选用就可以，有实力的同学还可以制作自己的镜像。在这里推荐一个兄弟根据Lean大神制作的镜像，功能上根据旁路网关用途做了简化，只包括了SSR plus,passwall和Clash三个关键插件，以及一些基础插件。完全胜任这里的功能需求而又没有多余的浪费。镜像名piaoyizy/openwrt-x86。

1. 开启网卡混杂模式（很多人都说这个是必须打开的，但个人经验证并不是必须，没开我也用的很好）

```sh
sudo ip link set enp2s0 promisc on
```

其中enp2s0是我的网卡名，根据跟人实际修改。

2. 创建虚拟vlan

```sh
docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.2 -o parent=enp2s0 macnet
```


其中192.168.1.0/24是我的内网地址，根据个人实际情况修改。192.168.1.2是我的主路由器地址，一样要改成你自己的。macnet是新建的macvlan名，可以随意取，但要记住，下条命令会用到。

3. 安装容器

```sh
docker run --restart always --name openwrt -d --network macnet --privileged piaoyizy/openwrt-x86
```

其中openwrt是创建的容器名称，个人喜好修改。macnet是上一步中创建的macvlan名称，必须与上一步保持一致。

4. 修改openwrt地址

通过docker ps命令查看openwrt容器ID

然后通过`docker exec -it ContainerID bash`进入openwrt 命令行，其中ContainerID为前面查到的容器ID

然后通过`vim /etc/config/netwrok `修改软路由IP地址。按`i`键开始输入以下内容：

```config
config interface 'lan'
option type 'bridge'
option ifname 'eth0'
option proto 'static'
option netmask '255.255.255.0'
option ip6assign '60'
option ipaddr '192.168.1.254'
option gateway '192.168.1.2'
option dns '192.168.1.2'
```

其中`option ipaddr`为软路由地址，注意不要与内网其他地址冲突；`option gateway`为内网网关，就是2步中的主路由地址，两者要一致；`option dns`选填，不加也可以。其他选项用这个例子中的就可以。

修改完毕后先按Esc键，再输入`:wq`并回车，就可以保存退出了。

5. 修改完成后通过`/etc/init.d/network restart `重启网络，这时就可以使用浏览器通过软路由地址192.168.1.254(上一步刚刚修改的地址)访问软路由了，密码是`password`。

6. 在网页端修改软路由配置，并将其他设备网关指向软路由就可以实现Magic上网了。旁路网关的设置方案网上很多，大家自行检索一下吧。我的方式是在主路由开启DHCP，将网关指向软路由，这样其他设备自动完成Magic上网。这种旁路网关的方式不用每个终端进行配置就可以实现Magic上网，而且一旦软路由出现问题，或者自己想折腾的时候，简单的只修改主路由的DHCP配置，将网关指向主路由自己，其他设备重连下WIFI或网络，就可以以普通的方式上网，对家人的影响可以忽略不计了。非常适合爱折腾的我自己。

## 其他附加功能安装

至于其他附加功能如网盘啥的，都通过Docker 实现，具体方法可以参考我的这篇文章软路由Openwrt还可以做什么

https://www.jianshu.com/p/82d4b580d24a