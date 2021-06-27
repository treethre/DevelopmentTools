# win10自带Hyper-V安装CentOS7及网络配置图文教程_操作系统_fengdong1996的博客-CSDN博客

原文链接： [blog.csdn.net](https://blog.csdn.net/fengdong1996/article/details/95041109)

# 开启win10自带的Hyper-v

### 1、查看是否已经开启了Hyper-V，步骤：开始 > 管理工具 > Hyper-V管理器.

这个一般都不会开启，所以不显示，需要我们手动设置出来。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f654ec9fd45e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2、打开 程序和功能 ，点击 "启用或关闭Windows功能 " 。

程序和功能就是卸载软件的地方，可以**右键开始**,选“**应用程序和功能**”打开，不行在控制面板找。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f654ecaf6c0e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 3、将Hyper的这几个都选中，点击 “确定”，需要重启电脑。

 

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f654ecb5274e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

有人 “**Hyper-V平台”** 这项肯定时黑色，并且其下的服务或监控程序是灰色选不了。无法选则需要我们去**BIOS中开启虚拟化技术**。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f654ecb69270?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

开启方式百度 **“BIOS开起虚拟化”** ，教程很多，根据自己的机型搜怎么进入bios，一般都是在开机时按F2或F2,不行就乱按吧，只要见了代V的选项,选为**Enable**就行，最后保存开机，开启了虚拟化技术后程序和功能里的Hyper-V所有选项就能选择开启了，然后保存退出**。这步一定要开，否则玩不了虚拟机的！！**

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f6550804bcb3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 4、重启后点击打开，开始 > 管理工具 > Hyper-V管理器，如下图。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f65509b2958c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

确定后如果有如下错需要去服务中开起相关服务。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f6551b9b7475?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

右键 **我的电脑 > 管理 > 服务和应用程序 > 服务。** 再去尝试上一步，如果还不行将有关服务全开。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f6551cad1443?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### **5、Hyper-V打开成功，可以在其上创建虚拟机了。**

到这步的伙伴离成功又进了一步，但是离成功还很远，到这里才是具备了在windows上安装虚拟机的资格,不是如下图的样子那就是没有真正打开Hyper管理器，在去设置BIOS吧。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f6552b08fc14?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 

--------------------------------Hyper-开启成功，开始安装虚拟-------------------------------

------

------

# 下载Contos7

阿里云下载：[mirrors.aliyun.com/centos/7/is…](http://mirrors.aliyun.com/centos/7/isos/x86_64/)

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f6553442e99a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

建议下载这个版本，900M最小安装。DVD版本较大，但是里面会有很多东西，包括图形化界面，但是图形界面不适用，又卡又占内存下载还慢。

## 网络准备：创建网络虚拟交换机。

### **1、点击 ："虚拟交换机管理器 " 进行创建交换机。**

这里提前创建交换机是 因为到会的安装过程中会有一步选择交换机，这里提前创建好，到会安装时直接选择，在安装完成后虚拟机直接就可以连接网络。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f6553da79911?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2、点击: 新建网络交换机 > 外部网络 > 创建虚拟交换机。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f6553dc8bcd9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 3、选择对应网卡，点击 "确定 " 完成创建交换机。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f6554b73fe71?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

----------------------------------准备过程结束，开始创建虚拟机----------------------------

------

## 创建linux虚拟机

### 1、点击：新建 > 虚拟机 ，进入向导。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f655521f56fe?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2、直接 下一步。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f6556198b2e8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 3、设置虚拟机名称、安装位置，完成后 下一步。

名称随便起，无所谓。安装位置就是在window中创建一个文件夹，在这里点击 “**浏览**” 选择这个文件夹，将来就会将linux装到这个文件夹中。也可以默认安装到C盘。

![ ](https://user-gold-cdn.xitu.io/2020/5/29/1725f65565e663e6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 3、设置虚拟机代数，默认第一代，点击 下一步。

这里也可以选择第二代，选择第二代后安装过程和第一代还是一样，唯一区别就是选择第二代在安装完后成后的初次开机时需要右键我们的虚拟机，选择 “**设置**” > "**安全**" 关闭一个选项，否则是启动不了linux的。但是选择第一代就没有这一步，安装完成直接开机，所以建议初次安装的选 第一代。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655727b956c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 4、设置虚拟机内存，全部默认 ，直接 **下一步**。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655799277c3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 5、设置网络，选择一个在准备过程中创建的虚拟交换机，点 下一步。

**根据自己使用的是线网络还是无线网络，选择对应的交换机，最后两个就是我们在"网络准备"步骤创建的网络交换机**。一定要选对了，不然在安装完虚拟机后连不上网，还要去重新设置网络，要知道linux默认是没有图形化界面的，就像windows的cmd一样，一个黑框，全靠敲命令，等到那时在去设置网络就难受了。所以我们在这里选择好，在安装系统时打开网络（初次安装系统时会有类似安装向导的界面），会省很大的事。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f65586df1a47?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 6、创建虚拟硬盘。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f6558f573955?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 7、选择自己下载的centos7系统，点"下一步"。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f6559170cd7a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 8、直接点击 "完成。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655996bf62a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

虚拟机创建完成后这里会多出来我们创建的虚拟机，他还正处于关机状态。双击打开

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655aa16c802?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

双击后出现如下界面，点击“启动"j进入linux系统，因为时新装的系统，初次进入需要安装设置。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655b63fc32a?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

提示：如果选了第二代的需要右键虚拟机 > 安全，取消这项，否则开不了机。选了第一代的不用管，可以正常开机安装系统。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655bee5d2d1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

------------------------安装设置完，开始真正的系统安装--------------------------

------

## 进入系统，开始初次进入安装。

### 1、启动后到如下图，选第一项，直接开始安装。

注意，这里是不能用鼠标的，用键盘控制光标。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655c04b4689?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 2、第一次进入系统会有如下安装引导。选择语言，拉到最下面选中文，然后点击继续。

提示，因为分辨率问题，下面有一部分展示不出来，需要将本窗口全屏就能全部展示。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655d0b1f049?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)，

### 3、点击：安装位置

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655d6dae2d6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655dd0a27cf?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 4、设置完安装位置后在点击设置网络。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655e20abf97?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655f72ae497?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655fe2299e9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1) ↵

 

完成后就从原来的未连接变成已连接，在本文最后面有通过命令方式修改。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f655ff13bc4f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### **5、再次全屏，下面有”开始安装“的按钮，点击开始安装**。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f6560732a8e2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### **6、设置root密码**

输入密码、确认密码，点击完成，这里提示不要用右边的数字小键盘，应为centos老是自动关闭数字键盘，安了半天发现数字键盘不开启，还是用键盘上面的数字键。接下来就是漫长的等待，

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f6561c784896?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

完成后重启。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f656200f9605?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 7、安装完成，登陆系统。

是不是有些惊讶，这就完了，桌面都没有，这怎么用！！其实虚拟机本来就不是让你像windows那样用的，我们一般买个服务器就是这样的，我们都是用windows远程连接到买的服务器，在windows下载图形化工具远程连接服务，远程控制，毕竟服务器贵啊，没必要装个不怎么用桌面在那里浪费资源。当然也有给centos装图形化界面的教程，桌面类似window，但是提示一下，那个东西超占内存，本来我们现在就是将linux装到windows下了，现在windows上的电脑管家显示内存占用五十多，当你在给linux安装一个桌面，你会发现那比windows费内存多了去了，我8G内存占用率直接九十多，而且那个桌面还超卡，没有什么实用价值。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f65623e626e6?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

最后在测试下网络是否开启。

```bash
ping www.baidu.com
```

没错，这就上了百度了，也没有界面。-_-

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f6562b58ca3f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)8、

### 结束语

windows安装虚拟机完成，我们可以开启centos7的**远程ssh**功能，用window去远程控制他，将他最小化放在后台，当他是远程的服务器，用windows下载安装远程控制软件，连接控制虚拟机。虽然contos7是黑屏无界面，但是有好多控制软件有是有界面的，我们给windows安装这类软件，远程连接虚拟机就可以用图形化界面操作虚拟机了。

------

### 设置静态网络

因为如果woem

**1、查看ip**

如果网络已联通，我们可以用 ip addr 命令查看自己当前的ip地址

```
ip addr复制代码
```

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f6563fc1b364?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**2、修改网络配置文件**

我的虚拟机只能输入vi才会打开**vi编辑器**进入配置文件，网上有人输入vim。最后面我的是eth0,有人是ens33。根据输入ip addr后显示的这个来![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f656434630b5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)。

注意：vi 后面有空格。

```bash
vi /etc/sysconfig/network-scripts/ifcfg-eth0
```

**vi编辑器简单介绍：**输入上面命令会进入到”**vi编辑器**”，vi编辑器刚进入时是不能编辑的，需要按字母"i"或"insert"键才开始进入编辑模式，使用上下左右键移动光标进行编辑。建议将虚拟机窗口全屏，进入编辑模式后左下角会有 "--  INSERT --"字样。当我们编辑完成后按键盘左上角的退出**Esc**键，接着输入字符 "**:wq!**" 回车即可保存退出。

将ONBOOT=no改为yes，设置网络为开机自启。将BOOTPROTO=dhcp改为static,由动态网络为静态。 并在后面增加几行内容： 

```bash
IPADDR=192.168.0.128   
NETMASK=255.255.255.0   
GATEWAY=192.168.0.1 
DNS1=192.168.0.1    
```

注意：

ip前三段要和自己windows主机一样，最后一段可以改成其他值；

子网掩码、和网关都和windows主机的一样;

dns1也要设置，值和网关的设置成一样的就行。

开始我没有设置dns就连不上网，设置后就可以了。

![img](https://user-gold-cdn.xitu.io/2020/5/29/1725f65647c89719?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**修改完网络配置后需要重启网络**

 

```bash
systemctl restart network.service
```

---------------------------centos7设置静态网络完成------------------------------------