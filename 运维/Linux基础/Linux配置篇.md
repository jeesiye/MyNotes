# 系统文件

|              |                    |                                                              |
| ------------ | ------------------ | ------------------------------------------------------------ |
|              |                    |                                                              |
|              |                    |                                                              |
|              |                    |                                                              |
| **软件安装** |                    |                                                              |
|              | yum配置            | `/etc/yum.cnf`                                               |
|              | yum镜像            | `/etc/yun.repo.d/Centos-Base.repo`                           |
| **网络相关** |                    |                                                              |
|              | 网卡               | `/etc/sysconfig/network-scripts/ifcfg-ens33`                 |
|              | 回环地址           | `/etc/sysconfig/network-scripts/ifcfg-lo`                    |
|              | 网络号             | `/etc/sysconfig/network`                                     |
|              | DNS                | `/etc/resolv.conf`                                           |
|              | 主机名             | `/etc/hosts`                                                 |
|              | 临时禁用IPv6       | `/proc/sys/net/ipv6/conf/all/disable_ipv6` <br />`/proc/sys/net/ipv6/conf/default/disable_ipv6` |
| **用户管理** |                    |                                                              |
|              | 用户信息           | `/etc/passwd`                                                |
|              | 用户密码           | `/etc/shadow`                                                |
|              | 用户组信息         | `/etc/group`                                                 |
|              | 用户组密码         | `/etc/gshadow`                                               |
|              | 默认用户登陆配置   | `/etc/login.defs`                                            |
|              | 默认用户添加配置   | `/etc/default/useradd`                                       |
|              | 默认用户主目录文件 | `/etc/skel/`                                                 |
|              | root权限管理       | `/etc/sudoers`                                               |
| **启动项**   |                    |                                                              |
|              | 开启启动项         | `/etc/inittab`                                               |
|              | target运行级别     | `ll /lib/systemd/system/runlevel*.target`                    |
|              |                    |                                                              |
|              | 开机分区挂载       | `/etc/fstab`                                                 |
| **临时文件** |                    |                                                              |
|              | 内核版本           | `/proc/version`                                              |
|              | 处理器信息         | `/proc/cpuinfo`                                              |
|              | 内存信息           | `/proc/meminfo`                                              |
|              | 文件系统           | `/proc/filesystems`                                          |
|              | 开机回环信息       | `/var/log/dmesg`                                             |
|              | 上线时间           | `/var/run/utmp`                                              |
|              |                    |                                                              |
|              |                    |                                                              |
|              |                    |                                                              |

# 常见服务

|      |                            |                  |
| ---- | -------------------------- | ---------------- |
|      |                            |                  |
|      | 网络管理(`setup`或`nmtui`) | `NetworkManager` |
|      | 网络服务                   | `network`        |
|      | 防火墙                     | `firewalld`      |
|      |                            |                  |
|      |                            |                  |

# 汉化相关

## man指令

- `yum install man-pages-zh-CN`  
- `find / -maxdepth 4 | grep -i man/zh_cn`  
- 编辑用户目录中的`.bashrc`文件,添加别名设置:`	alias cman='man -M /usr/share/man/zh_CN'`  
- 重新登陆terminal即可.

# 网路相关

## 静态IP地址

```sh
# ll /etc/sysconfig/network-scripts/ifcfg-ens33
TYPE=Ethernet
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.129
NETMASK=255.255.255.0
BROADCAST=192.168.1.255
NETWORK=192.168.1.0
GATEWAY=192.168.1.2
DNS1=180.76.76.76
DNS2=114.114.114.114
HWADDR=00:0C:29:D3:24:1B
NAME=ens33	
```

## IP别名

- 临时生效 
  - `ifconfig ens33 add 192.168.1.130 netmask 255.255.255.0 up`  
    `ifconfig ens33 del 192.168.1.130` 
  - `ip addr add 192.168.1.130/24 brd + dev ens33 label ens33:0`  
    `ip addr del 192.168.1.130/24 brd + dev ens33 label ens33:0` 
- 永久生效
  1. `cp /etc/sysconfig/network-scripts/ifcfg-ens33 /etc/sysconfig/network-scripts/ifcfg-ens33:0` 
  2. 使用vim编辑`name` `device` `ipaddr` 等字段
  3. `ifup ens33:0` 
  4. `ip addr show` 

## 代理转发

## 禁用IPv6

- 临时禁用
  1. `echo 1>/proc/sys/net/ipv6/conf/all/disable_ipv6` 
  2. `echo 1>/proc/sys/net/ipv6/conf/default/disable_ipv6` 

```sh
#!/bin/bash
ifcfg_file="/etc/sysconfig/network-scripts/ifcfg-ens33"
network_file="/etc/sysconfig/network"
sysctl_file="/etc/sysctl.conf"
readonly ifcfg_file network_file sysctl_file
cp $ifcfg_file ~/.ifcfg_ens.bak
cp $network_file ~/.network.bak
cp $sysctl_file ~/.sysctl.bak
sed -i 's/IPV6INIT=yes/IPV6INIT=no/g' $ifcfg_file
sed -e '1,$d' $network_file
echo 'NETWORKING_IPV6=no'>$network_file
sed -e '1,$d' $sysctl_file
echo -e 'net.ipv6.conf.all.disable_ipv6=1\nnet.ipv6.conf.default.disable_ipv6=1'>$sysctl_file
sysctl -p
service network restart
ip addr show
```



## 静态路由