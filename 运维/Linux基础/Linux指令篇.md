# 组合键

## 终端terminal

- `Tab` : 自动补全
- `Ctrl+c` : 终止
- `Ctrl+d` : 退出
- `shift+up/down` : 上下移动一行
- `shift+pgup/pgdown` : 上下移动一页

# 文件管理

## 基础操作

### 简易操作

- 目录

  - 切换 : `cd`   `cd /`   `cd ~`    `cd ..`   

  - 创建 : `mkdir dir`   `mkdir -p dir1/dir2/dir3`

  - 列表 : `ls -a`   `ls -l`   `ls -d`

  - 当前 : `pwd`

- 文本

  - 创建 : `touch 1.txt`   `>1.txt`

  - 类型 : `file 1.txt`
  - 查看 : `cat`   `head`   `tail`   `less`   `more`
  - 统计 : `wc 1.txt`   `wc -c 1.txt #bytes`   `wc -m 1.txt #chars`   `wc -l 1.txt #lines`   `wc -L 1.txt #lengths`   `wc -w 1.txt #words`
  - 排序 : `sort file`   `sort -r file #反序排列输出` 
  - 重点 : `awk`   `sed`   `find`   `grep` 
  - 了解 : `diff #比较`   `split #分割` 
  - 编辑: `vi`   `vim`   

- 复制移动删除链接

  - 复制 : `cp 1.txt 1.txt.cp`   `cp -r dir dir.cp`   `cp -l 1.txt hard #硬链接`   `cp -s 1.txt symbol #软链接`
  - 移动 : `mv 1.txt dir/`   `mv 1.txt 2.txt #更改文件名称`
  - 删除 : `rm -rf 1.txt`
  - 链接 : `ln file hard #硬链接`   `ln -s file symbol #软连接`   `ln -sf file symbol #链接存在直接覆盖`

- 其他

  -  日期和时间 : `date`   `date -u #(UTC+0)`
  - 日历 : `cal` 

### find查询

### grep过滤

### awk详解

### sed详解

## 压缩备份

### 压缩指令

#### 了解

- 格式 : `.gz` 
  指令 : `gzip`   `gunzip` 
  原因 : 对目录不生效
- 格式 : `.bz2` 
  指令 : `bzip2`   `bunzip2` 
  原因 : 对目录不生效

#### 掌握

- `zip` 压缩包 
  - 迭代打包 : `zip -r 压缩包 输入目录` 
  - 指定解压 : `unzip 压缩包 -d 输出目录` 
- `tar.gz`压缩包 
  - `-z #调用gzip或gunzip指令`   `-j #调用bzip2或bunzip2指令`  
    `-c #打包`   `-x #解压`  
    `-v #冗余`   `-f #指令存档,置于末尾`   `-C #指定输出目录` 
  - 打包 : `tar cvf 压缩包 输入目录`   `tar -cvf 压缩包 输入目录`  
    `tar zcvf 压缩包 输入目录` `tar -zcvf 压缩包 输入目录`  
  - 解压 : `tar xvf 压缩包`   `tar -xvf 压缩包 -C 输出目录`  
    `tar zxvf 压缩包`   `tar -zxvf 压缩包 -C 输出目录` 
- `tar.bz2` 压缩包
  - 打包 : `tar cvf 压缩包 输入目录`   `tar -cvf 压缩包 输入目录`  
    `tar jcvf 压缩包 输入目录`   `tar -jcvf 压缩包 输入目录` 
  - 解压 : `tar xvf 压缩包`   `tar -xvf 压缩包 -C 输出目录`  
    `tar jxvf 压缩包`   `tar -jxvf 压缩包 -C 输出目录` 
- `xz` 压缩包 
  - 非默认安装,执行`yum install xz` 
  - `-k #取消默认,保留原文件`   `-z #压缩`   `-d #解压` 
  - 打包 : `xz -z 输入文件`   `xz -z -k 输入文件` 
  - 解压 : `xz -d 压缩包`   `xz -d -k 压缩包` 

### 备份指令

#### cpio

- 备份和恢复,必须使用绝对路径!(该指令,在执行过程中,若采用相对路径,始终是相对路径)
- `-i #copy-in,备份`   `-o #copy-out,恢复`  
  `-c #使用ASCII模式,适合跨平台备份`   `-B #将默认块512,扩大十倍,加快速度`   `-d #目录不存在,则创建` 
- 备份 : `find /home/admin|cpio -ocB>admin.cpio` 
- 恢复 : `cpio -icd</root/admin.cpio` 

#### dd

- 暂定,不能操作目录
- `dd if=file of=file.dd` 

# 系统管理

## service操作

|      |                             |
| ---- | :-------------------------- |
| 启动 | `service firewalld start`   |
| 停止 | `service firewalld stop`    |
| 重启 | `service firewalld restart` |
| 状态 | `service firewalld status`  |

## systemctl操作

|          |                                  |
| -------- | -------------------------------- |
| 启动     | `systemctl start firewalld`      |
| 停止     | `systemctl stop firewalld`       |
| 重启     | `systemctl restart firewalld`    |
| 状态     | `systemctl status firewalld`     |
| 自启     | `systemctl enable firewalld`     |
| 禁用自启 | `systemctl disable firewalld`    |
| 是否自启 | `systemctl is-enabled firewalld` |
| 状态列表 | `systemctl list-units`           |
| 自启列表 | `systemctl list-unit-files`      |
| 关机     | `systemctl poweroff`             |
| 重启     | `systemctl reboot`               |
| 待机     | `systemctl suspend`              |
| 休眠     | `systemctl hibernate`            |

> 补充了解电源操作的指令
>
> `half`   `reboot`   `sync`   `init` 

## chkconfig操作

|          |                           |
| -------- | ------------------------- |
| 自启     | `chkconfig firewalld on`  |
| 禁用自启 | `chkconfig firewalld off` |
| 列表     | `chkconfig --list`        |

## 资源信息

- 系统信息 : `dmidecode`   (`yum install dmidecode`)
- 开机日志 : `dmesg` 
- PCI总线 : `lspci` 
- 系统版本
  - 内核信息 : `uname -a` 
  - 可读信息 : `lsb_release -a`   (`yum install redhat-lsb` | `yum install redhat-lsb-core`)
- 处理器相关 
  - 详细信息 : `cpuid|less`   (`yum install cpuid`)
  - 处理器个数 : `nproc`   (`yum install coreutils`)
  - 简要信息 : `lscpu` 
- 硬盘相关
  - 分区信息 
    - `lsblk #树形`   `lsblk -l #列表`   `lsblk -p #路径`   `lsblk -f #文件系统`   `lsblk -m #权限`   `lsblk -t #拓扑结构` 
    - `fdisk -l` 
    - `parted -l` 
  - 硬盘使用 : `df -hiT #h 可读; i inode节点; T 文件系统` 
  - 空间使用 
    - 可读的当前目录 : `du -sh` 
    - 可读的指定目录 : `du -sh /etc` 
    - 可读的指定深度 : `du -hd 2 /etc` 
  - 挂载卸载 : `mount`   `umount` 
- 内存相关
  - 内存空间 : `free`   `free -g #GB`   `free -m #MB`   `free -k #KB`   `free -b #B` 
  - 内存状态 : `vmstat`   `vmstat [间隔] [次数]`   `vmstat -d #所有硬盘`   `vmstat -w #等宽输出`   `vmstat -D #简要信息`   `vmstat -p /dev/sda1 #分区统计` 
- 上线时间 : `uptime` 
- 用户统计 : `who`   `w`   `last` 
- 查看用户和用户组的id  : `id` 
- 资源管理器 : `top` 
- 了解 : `fsck #文件系统修复`   `eject #拔出设备` 

## 进程管控

进程查看 : `ps aux #BSD语法`   `ps -ef #POSIX语法` 

# 用户管理

## 用户

- 切换用户 : `su - #切换root`   `su hack` 
- 修改密码 : `passwd #更改当前用户密码`   `passwd hack #更改指定用户密码,root权限` 
- 默认添加用户配置项 : `useradd -D` 
- 添加用户 : `useradd hack` 
- 删除用户 : `userdel hack`   `userdel -r hack #删除主目录和邮箱目录` 
- 配置用户 : `usermod -g grp1 hack #更改所属组`   `usermod -G grp2 hack #添加附属组` 

## 用户组

## 权限

# 软件管理

## rpm详解

- 查询
  - 是否存在 : `rpm -q lrzsz-0.12.20-36.el7.x86_64.rpm`   `rpm -qa|grep lrzsz` 
  - 指令所属 : `rpm -qf /usr/bin/ls`
  - 安装目录 : `rpm -ql lrzsz` 
  - 安装信息 : `rpm -qi lrzsz`
  - 说明文档 : `rpm -qd lrzsz` 
  - 开发软件 : `rpm -qg 'Development/Languages'` 
- 安装
  - 依赖 : `rpm -ivh lrzsz-0.12.20-36.el7.x86_64.rpm` 
  - 不依赖 : `rpm -ivh lrzsz-0.12.20-36.el7.x86_64.rpm --nodeps` 
- 验证
  - 状态 : `rpm -Vp lrzsz-0.12.20-36.el7.x86_64.rpm` 
  - 文件 : `rpm -Vf lrzsz-0.12.20-36.el7.x86_64.rpm` 
  - GPG签名 : `rpm -K lrzsz-0.12.20-36.el7.x86_64.rpm` 
- 其他
  - 更新 : `rpm -Uvh lrzsz-0.12.20-36.el7.x86_64.rpm`
  - 删除 : `rpm -e lrzsz-0.12.20-36.el7.x86_64.rpm` 

## yum详解

- `install & localinstall` 
  -  `yum install httpd` 
  -  `yum install httpd --downloadonly --downloaddir=/home/admin/rpms` 
  -  `yum localinstall lrzsz-0.12.20-36.el7.x86_64.rpm` 
- `update & upgrade` 
  - `yum update`   
  - `yum update all`   `yum check-update`   `yum upgrade`   
- `info & list`   
  - `yum info httpd`  
    `yum info`   `yum info all`   `yum info installed`   `yum info extras`   `yum info update`   
  - `yum list httpd`  
    `yum list`   `yum list all`   `yum list installed`   `yum list extras`   `yum list update`   
- `search` 
  - `yum search httpd`  
    `yum provides httpd` 
- `clean` 
  - `yum clean`  
  - `yum clean all`   `yum clean packages`   `yum clean headers`   `yum clean oldheaders`   
- `remove`   
  - `yum remove httpd`
  - `yum autoremoe`   
- `yum mackcache`   
- `yum repolist`   `yum repoinfo`   `yum history`   

# 网络管理

## 网卡

- 地址查看 : `ip addr show`   `ifconfig` 
- 启动断开 
  - 推荐 : `ifup ens33`   `ifdown ens33` 
  - 不推荐 : `ifconfig ens33 up`   `ifconfig ens33 down`  
    `ip link set ens33 up`   `ip link set ens33 down` 
- mac地址 : `nmcli dev show #centos7`   `nmcli dev list #centos6`    (`yum install NetworkManager`)
- uuid值 : `nmcli con show #centos7`   `nmcli con list #centos6` 
- 添加IP临时别名 : `ifconfig ens33 add 192.168.1.131 netmask 255.255.255.0 up`  
  `ip addr add 192.168.1.131/24 brd + dev ens33 label ens33:0` 
- 删除IP临时别名 : `ifconfig ens33 del 192.168.1.131`  
  `ip addr del 192.168.1.131/24 brd + dev ens33 label ens33:0` 

## 端口

- 查看 : `netstat -natup` 
- 测试 : `telnet 192.169.1.129 22` 

## 路由

- 查看 : `route -n`   `netstat -rn` 
- 跟踪 : `traceroute` 

## 下载

- url下载 : `wget url地址`   (`yum install wget`)
- 主机传输 : `scp` 
- 文本浏览器 : `elinks`   (`yum install elinks`)