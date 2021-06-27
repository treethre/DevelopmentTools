## CentOS最小化安装后的一些操作

### 安装ifconfig

最小化安装CentOS7后，想查看我的IP，发现 ifconfig命令不存在。

在最小化的CentOS7中，查看网卡信息

```
ip addr
```

ifconfig命令依赖于net-tools，为了方便起见 我们还是启用ifconfig 命令。

```
yum install -y net-tools
```

### 关闭防火墙

```
systemctl stop firewalld
systemctl disable firewalld
```

### 关闭selinux

```
setenforce 0
sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config
```

### 安装wget

```
yum install -y wget
```

### 更换yum源

备份系统旧配置文件

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
```

获取yum配置文件到/etc/yum.repos.d/

```
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

更新缓存

```
yum clean all
yum makecache
```

### 安装epel源

EPEL (Extra Packages for Enterprise Linux) 是由 Fedora Special Interest Group 为企业 Linux 创建、维护和管理的一个高质量附加包集合适用于但不仅限于 Red Hat Enterprise Linux (RHEL), CentOS, Scientific Linux (SL), Oracle Linux (OL)

备份系统旧配置文件

```
mv /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel.repo.bak
```

获取epel配置文件到/etc/yum.repos.d/

```
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

更新缓存

```
yum clean all
yum makecache
```

### 安装unzip

```
yum install -y unzip
```

### 安装命令自动补全

Centos7在使用最小化安装的时候，没有安装自动补全的包，需要自己手动安装。

```
yum install -y  bash-completion
```

安装好后，重新登陆即可（刷新bash环境）

### 免密登陆

配置一台Linux到另一台Linux的免密登录

| A    | 192.168.1.1 | 客户机 |
| :--- | :---------- | :----- |
| B    | 192.168.1.2 | 目标机 |

要达到的目的：A机器ssh登录B机器无需输入密码

加密方式选 rsa|dsa均可以，默认dsa

两种方法都可行

#### 第一种

这种情况适用于自己有远程主机的账号密码

##### 复制公钥到远程主机

在192.168.1.1上执行命令：

（命令1有交互按自己的需求填写，简单的就全部直接回车）

（命令2需要输入192.168.1.2 的 root 密码）

```
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.1.2 #复制公钥到远程主机
```

#### 第二种

这种情况适用于把公钥交给服务器管理员

##### 登录A机器

##### 生成公钥文件和私钥文件

```
ssh-keygen -t [rsa|dsa]
```

将会生成密钥文件`id_rsa`,`id_rsa.pub`或`id_dsa`,`id_dsa.pub`

##### 复制公钥文件

将`.pub`文件复制到B机器的`.ssh`目录，并执行以下命令

```
cat id_rsa.pub >> ~/.ssh/authorized_keys
#普通用户之间免密登陆，需要执行以下命令
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

##### 大功告成

从A机器登录B机器的目标账户，不再需要密码了（直接运行`ssh 192.168.1.2`）

### 安装GNOME图形界面

安装图形用户接口`X Window System`，在命令窗口输入：

```
yum groupinstall "X Window System"
```

安装完成会提示`complete`!

提示：`X Window System`本身是一个非常复杂的图形化作业环境，我们可以将它分成3个部分，分别是`X Server`、`X Client`和`X Protocol`。`X Server`主要是处理输入输出的信息，`X Client`执行大部分应用程序的运算功能，`X Protocol`则是建立`X Server`和`X Client`的沟通管道。

`X Window`通过软件工具及架构协议来建立操作系统所用的图形用户界面，此后则逐渐扩展适用到各形各色的其他操作系统上，几乎所有的操作系统都能支持与使用`X Window`，`GNOME`和`KDE`也都是以`X Window`为基础建构成的。

安装图形用界面`gnome`，在命令窗口输入:

```
yum groupinstall "GNOME Desktop"
```

安装完成，同样会提示`compete`!

提示：检查已经安装的软件以及可以安装的软件，用命令`yum grouplist`

同样在root用户权限下，设置centos系统默认的启动方式，输入命令如下：

`systemctl set-default multi-user.target` //设置成命令模式

`systemctl set-default graphical.target` //设置成图形模式

> 注意：centos7和centos6设置方式不同！

重启系统即可

### 设置IP地址

快速设置方法：

实际一行命令就搞定

查看一下网卡名称

```
ip a
```

设置ens33网卡ip地址

```
ip addr 192.168.10.230/24 dev ens33
```

其它快速设置参考命令：

显示网卡IP信息

```
ip addr
```

显示ens33网卡IP信息

```
ip addr show ens33
```

开启或关闭ens33网卡

```
ip link set ens33 up/down
```

设置ens33网卡IP地址192.168.10.230

```
ip addr add 192.168.10.230/24 dev ens33
```

删除ens33网卡IP地址192.168.10.230

```
ip addr del 192.168.10.230/24 dev ens33
```

查看路由信息,也可以使用ip route命令

```
ip route list
```

设置默认网关，也可以在后面加上dev ens33

```
ip route add default via 192.168.10.1
ip route add default via 192.168.10.1 dev ens33
```

删除默认网关，也可以直接写`ip route del default`

```
ip route del default via 192.168.10.1
```

设置192.168.10.0网段的网关为192.168.10.1,数据走eth0接口

```
ip route add 192.168.10.0/24 via 192.168.10.1 dev eth0
```

删除192.168.100.123.0/24网段的路由

```
ip route del 192.168.10.0/24
```

### 修改启动级别

获取当前启动级别

```
systemctl get-default
```

更改启动模式为多用户命令模式

```
systemctl set-default multi-user.target
rm '/etc/systemd/system/multi-user.target'
ln -s '/usr/lib/systemd/system/multi-user.target' '/etc/systemd/system/multi-user.target'
```

更改启动模式为图像模式

```
systemctl set-default graphical.target
rm '/etc/systemd/system/default.target'
ln -s '/usr/lib/systemd/system/graphical.target' '/etc/systemd/system/default.target'
```

### 字符编码

#### 查看当前locale设置

```
locale
```

#### 设置系统locale

此处以`zh_CN.UTF-8`为例

编辑文件`/etc/profie`

```
vi /etc/profile
export LANG=zh_CN.UTF-8
source /etc/profile
```