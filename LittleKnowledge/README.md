# “Linux”并不是Linux系统，完整的Linux系统由8部份组成

Linux发行版本并不是只有Linux内核。Linux发行版本都包含了其它重要的软件，比如Grub bootloader（Grub引）, Bash shell, GNU shell utilities, daemons, X.org graphical server, a desktop environment等

以上不同程序属于不同的开发者或组织，由Linux发行版组合在一起，形成一个完整的Linux系统。

## Bootloader

当你启动电脑时，计算机BIOS or UEFI固件从启动设备加载软件。与任何操作系统一起加载的第一个程序是引导加载程序。对于Linux来说，一般就是Grub引导程序

如果安装的是双系统，Grub会提供菜单选项，让用户做选择。比如：如果在双引导配置中安装了Linux，则可以在引导时选择Linux或Windows。

如果只是安装了单个Linux系统，Grub会直接启动该Linux系统。GRUB处理如何启动Linux，输出命令行选项，并允许您以其他方式启动Linux以进行故障排除。如果没有引导加载程序，Linux发行版就无法引导。

## The Linux Kernel

Grub引导的主要程序是Linux内核（The precise piece of software Grub boots is the Linux kernel.）而该Linux内核才是“Linux”这个单词表达的确切意义。内核是操作系统的核心，但不代表一个完整的操作系统。Linux内核管理CPU，内存，I/O设备（键盘，鼠标，显示设备）。因为内核直接与硬件交互，所以许多硬件驱动程序是Linux内核的一部分，并在和内核一起运行。

其它的软件运行在内核上。内核是其它软件基础，用来于硬件进行直接交互。并且内核提供了硬件的抽像，屏蔽了硬件的区别，这样系统上的其它部分可以很少的考虑硬件之间的差别。Windows现在用的是Windows NT内核，Linux用的是Linux内核


## Daemons

守护进程，本质上是后台进程。他们经常启动作为操作系统的引导过程的一部分，他是在内核进程启后和图形登录屏幕前被启动。Windows称其为“服务”，在类UNIX系统中叫“Daemons”。

例如，管理周期任务的Cron“守护”进程。另一个传统的syslogd是管理你的系统日志守护进程。各类服务如sshd,会以守护进程的方式运行在后台。这确保他们总是与听力再运行远程连接。

守护进程是一些必要程序，运行在后台，但它们是系统级进程一般用户无感知。

## The Shell

大部分Linux系统使用Bash Shell。 Shell提供命令行交互接口给用户，允许用户通过文本交互控制电脑。当然也能运行Shell脚本（脚本是命令集合，并且是脚本内定义顺序执行的文本）

即使我们使用的是图形桌面，Shell也会运行，可以在后台使用。

## Shell Utilities

Shell只提供了基本的命令，比如CP，ls,rm并没有提供，而这些是属于GNU Core Utilities包中的

Linux系统如果没有这些得要的工具是没有办法工作。事实上，Bash Shell本身是GNU项目的组成部分。

但是并不是所有的Shell工具和命令行程序是由GNU项目开发的。

## X.org Graphical Server

Linux的图形桌面也不属于Linux内核。被称为“X Server”，“X window system”是它的一个实现。

现在流行的“X Server”（图形服务）是X.org。我们所看到的图形登录界面或图形桌面就是X.org在后台做的功能支撑。X.org运行一套完整的图形系统，该系统与电脑的声卡，显示设备，鼠标和其它设备进行交互

## Desktop Environment

用户真正在桌面系统用到的叫桌面环境。比如Ubuntu的Unity桌面环境，Fedora的GNOME，Kubuntu的KDE，Mint的Cinnamon或MATE。这些环境提供你开机所能开到的一切，比如桌面背景，面板，窗口标题栏和边框。

作为一个桌面环境整体一起发布的还有一些自己的工具组件。例如GNOME和Unity包函了Nautilus文件管理系统。KDE提供了Dolphin文件管理系统

## Desktop Programs

但并不是所有的桌面程序都属于桌面环境。比如，FireFox和Chrome就不区分桌面环境，可以运行在任何的桌面环境上。OpenOffice.org也是一组程序，并不绑定到任何桌面环境，用户可以自行安装。

我们可运行任何Linux桌面程序到任何一个桌面环境。但如果是针对于某个桌面环境定制桌面程序则需要伴随安装额外的库和启动相关的辅助进程。比如，GNOME的Nautilus文件管理程序到KDE桌面环境上，我们就要安装GNOME库，可能还要启动GNOME桌面环境进程才能在KDE环境中正确的运行Nautilus文件管理程序

[原文](https://www.howtogeek.com/177213/linux-isnt-just-linux-8-pieces-of-software-that-make-up-linux-systems/)


```
不同的Linux distros其实就是用一不同的软件选择策略，有的不包含闭源程序，有的为了方便用户体验选择包括闭源程序。

用户实际中使用到的明显感受就是，包管理程序不同（yum or apt），配置方式不同(不同的桌面环境导致)，默认安装的应用软件会有区别。仅此而以！

下面是一段对Linux distros的描述
Linux works differently. The Linux operating system isn’t produced by a single organization. Different organizations and people work on different parts. There’s the Linux kernel (the core of the operating system), the GNU shell utilities (the terminal interface and many of the commands you use), the X server (which produces a graphical desktop), the desktop environment (which runs on the X server to provide a graphical desktop), and more. System services, graphical programs, terminal commands – many are developed independently from another. They’re all open-source software distributed in source code form.

If you wanted to, you could grab the source code for the Linux kernel, GNU shell utilities, Xorg X server, and every other program on a Linux system, assembling it all yourself. However, compiling the software would take a lot of time – not to mention the work involved with making all the different programs work properly together.

Linux distributions do the hard work for you, taking all the code from the open-source projects and compiling it for you, combining it into a single operating system you can boot up and install. They also make choices for you, such as choosing the default desktop environment, browser, and other software. Most distributions add their own finishing touches, such as themes and custom software – the Unity desktop environment Ubuntu provides, for example.
```

# PPA是什么

PPA软件源，全称是Personal Package Archives。虽然Ubuntu官方软件仓库尽可能囊括所有的开源软件，但仍有很多软件包由于各种原因不能进入官方软件仓库。为了方便Ubuntu用户使用，launchpad.net提供了个人软件包集，即PPA，允许用户建立自己的软件仓库，通过Launchpad进行编译并发布为2进制软件包，作为apt/新立得源供其他用户下载和更新。PPA也被用来对一些打算进入Ubuntu官方仓库的软件，或者某些软件的新版本进行测试。

Launchpad是Ubuntu母公司canonical有限公司所架设的网站，是一个提供维护、支援或联络Ubuntu开发者的平台。

可以在launchpad(https://launchpad.net/ubuntu/+ppas)平台上直接搜索相关的软件名称以便获得相关源地址，点击查看链接地址。例如搜索redis-server结果如下：

![launchpad](pic/launchpad_ppas.png)

点击搜索结果redis-server，查看安装方法：

![launchpad_ppa_detail.png](pic/launchpad_ppa_detail.png)

```bash
sudo add-apt-repository ppa:jerrywei/redis-server
sudo apt-get update
```

上面第一条命令会在/etc/apt/sources.list.d下创建.list文件。如下图所示:

![repository_sample](pic/repository_sample.png)

jerrywei-ubuntu-redis-server-xenial.list内容如下：

```
learlee@learleePC:~$ sudo more /etc/apt/sources.list.d/jerrywei-ubuntu-redis-server-xenial.list 
deb http://ppa.launchpad.net/jerrywei/redis-server/ubuntu xenial main
# deb-src http://ppa.launchpad.net/jerrywei/redis-server/ubuntu xenial main
```

也可以在/etc/source.list中直接添加以下地址：

```
deb http://ppa.launchpad.net/jerrywei/redis-server/ubuntu xenial main
# deb-src http://ppa.launchpad.net/jerrywei/redis-server/ubuntu xenial main
```

**删除软件库**

```
Step 1: sudo add-apt-repository -r ppa:user/ppa-name
Step 2: 进入 /etc/apt/sources.list.d 文件夹，删除对应的源文件。
```

# Ubuntu简单添加开机启动

有的时候按装了一个应用程序，我们需要其开机的时候就启动。以前的方式是在/etc/profile里添加，现在Ubuntu可以使用Startup Applications应用把要添加的应用添加进去 即可。做到了所见即所得。如下图所示。在应用中搜索Startup Applications即可以。使用方式：点击Add按键会弹出添加程序对话框，在Name和comment里说明是什么即可。把启动命令写入Command里。

注：如果不知道命令存方位置可以通过”whereis commandName”来获取，如下所示，albert的启动命令在/usr/bin/albert

```bash
learlee@learleePC:~$ whereis albert
albert: /usr/bin/albert /usr/lib/albert /usr/share/albert
```

StartupAPP提示框如下：

![StartupApp](pic/StartupApp.png)


# selinux

安全增强型 Linux（Security-Enhanced Linux）简称 SELinux，它是一个 Linux 内核模块，也是 Linux 的一个安全子系统。SELinux 主要由美国国家安全局开发。2.6 及以上版本的 Linux 内核都已经集成了 SELinux 模块。

**SELinux 的作用**

1. SELinux 主要作用就是最大限度地减小系统中服务进程可访问的资源（最小权限原则）。
2. 在没有使用 SELinux 的操作系统中，决定一个资源是否能被访问的因素是：某个资源是否拥有对应用户的权限（读、写、执行）。只要访问这个资源的进程符合以上的条件就可以被访问。而最致命问题是，root 用户不受任何管制，系统上任何资源都可以无限制地访问。这种权限管理机制的主体是用户，也称为自主访问控制（DAC）。
3. 强制访问控制（MAC）。决定一个资源是否能被访问的因素除了上述因素之外，还需要判断每一类进程是否拥有对某一类资源的访问权限。这样一来，即使进程是以 root 身份运行的，也需要判断这个进程的类型以及允许访问的资源类型才能决定是否允许访问某个资源。进程的活动空间也可以被压缩到最小。

DAC MAC区别如下图

![selinuxdacmac](pic/selinuxdacmac.jpeg)

**SELinux 的工作模式**

SELinux 有三种工作模式，分别是：
1. enforcing：强制模式。违反 SELinux 规则的行为将被阻止并记录到日志中。
2. permissive：宽容模式。违反 SELinux 规则的行为只会记录到日志中。一般为调试用。
3. disabled：关闭 SELinux。

**SELinux 工作流程**

![](pic/selinuxworkflow.jpeg)


**关闭方式**

1. 使用setup工具 进入图形化关闭
或者
2. 向/etc/sysconfig/selinux 文件加入SELINUX=disabled


```
etenforce是Linux的selinux防火墙配置命令 执行setenforce 0 表示关闭selinux防火墙。

setenforce命令是单词set（设置）和enforce(执行)连写，另一个命令getenforce可查看selinux的状态。 
```

# ifconfig中lo、eth0、br0、wlan0

## lo 回环接口

```
lo Link encap:Local Loopback
inet addr:127.0.0.1 Mask:255.0.0.0
```

虚拟网络接口:并非真实存在，并不真实地从外界接收和发送数据包，而是在系统内部接收和发送数据包，因此虚拟网络接口不需要驱动程序。

为什么会有该接口？
如果包是由一个本地进程为另一个本地进程产生的, 它们将通过外出链的lo接口,然后返回进入链的lo接口

## eth0 以太网接口

```
eth0 Link encap:Ethernet HWaddr 04:7d:7b:7e:6d:19
inet addr:192.168.1.106 Bcast:192.168.1.255 Mask:255.255.255.0

HW代表hardware
```

以太网接口与网卡对应，每个硬件网卡(一个MAC)对应一个以太网接口，其工作完全由网卡相应的驱动程序控制。如果物理网卡只有一个，而却有eth1，eth2等，则可能存在无线网卡或多个虚拟网卡，虚拟网卡由系统创建或通过应用层程序创建，作用与物理网卡类似。

## br0 网桥接口

```
br0 Link encap:Ethernet HWaddr a2:d3:29:ba:51:4b 
```

网桥是一种在链路层实现中继，对帧进行转发的技术，根据MAC分区块，可隔离碰撞，将网络的多个网段在数据链路层连接起来的网络设备。br0可以将两个接口进行连接，如将两个以太网接口eth0进行连接，对帧进行转发。

## wlan0 无线接口

```
wlan0 Link encap:Ethernet HWaddr 9c:b7:0d:c0:0b:36
inet addr:192.168.1.115 Bcast:192.168.1.255 Mask:255.255.255.0
```

无线网卡对应的接口，无线网卡也需要对应的驱动程序才能工作。

## 控制命令

ifconfig

功能说明：显示或设置网络设备

语　法：ifconfig [网络设备][down up -allmulti -arp -promisc][add<地址>][del<地址>][<硬件地址>] [media<网络媒介类型>][mem_start<内存地址>][metric<数目>][mtu<字节>][netmask<子网掩码>][tunnel<地址>][-broadcast<地址>] [-pointopoint<地址>]

**补充说明：ifconfig可设置网络设备的状态，或是显示目前的设置。**

参数：
```
[网络设备] 网络设备的名称。

down 关闭指定的网络设备。

up 启动指定的网络设备。

-arp 打开或关闭指定接口上使用的ARP协议。前面加上一个负号用于关闭该选项。

-allmuti 关闭或启动指定接口的无区别模式。前面加上一个负号用于关闭该选项。

-promisc 关闭或启动指定网络设备的promiscuous模式。前面加上一个负号用于关闭该选项。

add<地址> 设置网络设备IPv6的IP地址。

del<地址> 删除网络设备IPv6的IP地址。

media<网络媒介类型> 设置网络设备的媒介类型。

mem_start<内存地址> 设置网络设备在主内存所占用的起始地址。

metric<数目> 指定在计算数据包的转送次数时，所要加上的数目。

mtu<字节> 设置网络设备的MTU。

netmask<子网掩码> 设置网络设备的子网掩码。

tunnel<地址> 建立IPv4与IPv6之间的隧道通信地址。

-broadcast<地址> 将要送往指定地址的数据包当成广播数据包来处理。

-pointopoint<地址> 与指定地址的网络设备建立直接连线，此模式具有保密功能。
```

## ip地址及子网掩码换算，子网划分

一般都会用到子网划分，来解决网络风暴的产生。也有通过子网划分来解决组播和广播的优化网络的。

IP地址划分，我们一般需要计算如下数据：
- 具体的子网掩码
- 子网数
- 可用的主机数
- 最大可容纳主机数
- 网络地址
- 广播地址是连接网络线路的一种装置

看一下示例 192.168.1.53/27。如何计算上面的数据呢？

Step1 计算子网掩码

平时如果我们给出的子网掩码是255.255.255.0,那么二进制下是11111111.11111111.11111111.00000000。这样前面24位表示网络号，后面的8位表示主机数，现在返过来看192.168.1.53/27，其中27表示的是网络号数的个数，指从左向右数。那么转换后就是11111111.11111111.11111111.11100000。即主机数只有5位

Step2 可用的主机数和最大可容纳主机数

192.168.1.53/27的子网掩码11111111.11111111.11111111.11100000所有可用的主机数为主机号所剩下的5位掩码所能表示的组合个数，即最多可容纳主机数为32（2的5次幂：２^5）;可用主机数为30（规定每个子网的第一个IP地址为网段地址，最后一个IP地址为广播地址，都不可用：２^5-2）

Step3 具体的子网掩码

192.168.1.53/27的子网掩码11111111.11111111.11111111.11100000所以转换成十进制是255.255.255.224

Step 4 子网数
准确说把C类网可以划分成几个子网。从二进制子网掩码的最后8位可以看到网络号只占三位，所以可以分割出来的C类子网是8个（2的3次方：２^3）

Step 4 网络地址和广播地址

192.168.1.53/27从子网来看是属于第二个子网的主机地址：
- 第一个子网是从192.168.1.0--192.168.1.31(共32个:192.168.1.00000000--192.168.1.00011111)
- 第二个子网是从192.168.1.32--192.168.1.63(共32个192.168.1.00100000--192.168.1.00111111)
- 依次类推

定每个子网的第一个IP地址为网段地址，最后一个IP地址为广播地址，所以网络地址是192.168.1.32广播地址是192.168.1.62

如果知道要建设可容纳888个主机数的网络，在192.168.0.0网段,如何设计子网掩码呢，计算网络地址和广播地址？

1. 从可用主机数:２^n – 2 =X 可以推出２^n – 2 ≥ 888所以n必须是10才能满足
2. 从主机数来反推掩码，主机地址位为0,所以需要10个0,即掩码为11111111.11111111.11111100.00000000
3. 这个实际上１０２２个，满足我们的实际需要。把上面的二进制换算成10进制是255.255.252.0
4. 网络地址，如果我们以192.168开始，那第从0开始的即查网络地址，即192.168.0.0
5. 广播地址是最后一位即192.168.00000011.11111111-->192.168.3.255
6. 最终我们可以使用192.168.0.0/22来描述该网段
   
```
子网掩码：由连续的1和0组成，连续的1表示网络地址，连续的0表示主机地址，通过0的个数可以计算出子网的容量（子网中主机的IP地址范围）。用子网掩码与IP地址与得出的网络地址一样即认为是同一个子网

网络地址：网络地址是识别网络ID用的,是用于隔离主机地址的，通俗的说电话的区号就是来隔离不同城市的电话号码的，有了网络地址就可以很好的对不同环境、不同领域、不同地理环境等主机地址的规划和管理。如192.168.1.0说明该网段属于192.168.1的段。属于不可用IP

广播地址：顾名思义是对网路上所有的ip地址进行广播自己的地址信息，广播又分为网内广播和网段广播，例如192.168.1.255，这就是你一个广播地址，对192.168.1这个网络的所有主机地址进行广播，192.168.255.255，这个就是对整个c类网段的广播，255.255.255.255，这个就不得了了，是对整个互联网的广播。在使用TCP/IP 协议的网络中，主机标识段host ID 为全1 的IP 地址为广播地址。
```

[引用](https://jingyan.baidu.com/article/ae97a646d936ddbbfd461d02.html)


# $() ${}区别

\$( )中放的是命令，相当于` `，例如todaydate=\$(date +%Y%m%d)意思是执行date命令，返回执行结果给变量todaydate，也可以写为todaydate=`date +%Y%m%d`；

\${ }中放的是变量，例如echo \${PATH}取PATH变量的值并打印，也可以不加括号比如\$PATH。

并且以上两个方式都可以在CLI中直接使用比如

```bash
chown $(id -u):$(id -g) $HOME/.kube/config

#$HOME不能写成$(HOME)
$ $(HOME)
HOME: command not found
```

# openssl 非对称加密算法RSA命令详解

非对称加密算法也称公开密钥算法，其解决了对称加密算法密钥分配的问题，非对称加密算法基本特点如下：

1. 加密密钥和解密密钥不同
2. 密钥对中的一个密钥可以公开
3. 根据公开密钥很难推算出私人密钥

用途：可用户数字签名、密钥交换、数据加密。但是由于非对称加密算法较对称加密算法加密速度慢很多，故最常用的用途是数字签名和密钥交换。

目前常用的非对称加密算法有```RSA, DH```和```DSA```三种，但并非都可以用于密钥交换和数字签名。而是RSA可用于数字签名和密钥交换，DH算法可用于密钥交换，而DSA算法专门用户数字签名。openssl支持以上三种算法，并为三种算法提供了丰富的指令集。

**抛开数字签名和密钥交换的概念，实质上就是使用公钥加密还是使用私钥加密的区别。所以我们只要记住一句话：“公钥加密，私钥签名”。**

- 公钥加密：用途是密钥交换，用户A使用用户B的公钥将少量数据加密发送给B，B用自己的私钥解密数据

- 私钥签名：用途是数字签名，用户A使用自己的私钥将数据的摘要信息加密一并发送给B，B用A的公钥解密摘要信息并验证

## RSA算法相关指令及用法

opessl中RSA算法指令主要有三个

指令 | 功能
:---|:--- 
genrsa | 生成并输入一个RSA私钥
rsa | 处理RSA密钥的格式转换等问题
rsautl | 使用RSA密钥进行加密、解密、签名和验证等运算

### genrsa指令说明

```bash
$ openssl genrsa -
usage: genrsa [args] [numbits]                                                     //密钥位数，建议1024及以上
 -des            encrypt the generated key with DES in cbc mode                    //生成的密钥使用des方式进行加密
 -des3           encrypt the generated key with DES in ede cbc mode (168 bit key)  //生成的密钥使用des3方式进行加密
 -seed
                 encrypt PEM output with cbc seed                                  //生成的密钥还是要seed方式进行
 -aes128, -aes192, -aes256
                 encrypt PEM output with cbc aes                                   //生成的密钥使用aes方式进行加密
 -camellia128, -camellia192, –camellia256 
                 encrypt PEM output with cbc camellia                              //生成的密钥使用camellia方式进行加密
 -out file       output the key to file                                           //生成的密钥文件，可从中提取公钥
 -passout arg    output file pass phrase source                                    //指定密钥文件的加密口令，可从文件、环境变量、终端等输入
 -f4             use F4 (0x10001) for the E value                                  //选择指数e的值，默认指定该项，e值为65537 -3              use 3 for the E value                                             //选择指数e的值，默认值为65537，使用该选项则指数指定为3
 -engine e       use engine e, possibly a hardware device.                         //指定三方加密库或者硬件
 -rand file:file:...
                 load the file (or the files in the directory) into                //产生随机数的种子文件
                 the random number generator
```

可以看到genrsa指令使用较为简单，常用的也就有指定加密算法、输出密钥文件、加密口令。我们仅举一个例子来说明

```bash
/*
 * 指定密钥文件rsa.pem
 * 指定加密算法aes128
 * 指定加密密钥123456
 * 指定密钥长度1024
 **/
$ openssl genrsa -out rsa.pem -aes128 -passout pass:123456 1024
```

### rsa指令说明

rsa指令用户管理生成的密钥，其用法如下:

```bash
 openssl rsa -
unknown option -
rsa [options] <infile >outfile     
where options are
 -inform arg     input format - one of DER NET PEM                      //输入文件格式，默认pem格式
 -outform arg    output format - one of DER NET PEM                     //输入文件格式，默认pem格式
 -in arg         input file                                             //输入文件
 -sgckey         Use IIS SGC key format                                 //指定SGC编码格式，兼容老版本，不应再使用
 -passin arg     input file pass phrase source                          //指定输入文件的加密口令，可来自文件、终端、环境变量等
 -out arg        output file                                            //输出文件
 -passout arg    output file pass phrase source                         //指定输出文件的加密口令，可来自文件、终端、环境变量等
 -des            encrypt PEM output with cbc des                        //使用des加密输出的文件
 -des3           encrypt PEM output with ede cbc des using 168 bit key  //使用des3加密输出的文件
 -seed           encrypt PEM output with cbc seed                       //使用seed加密输出的文件
 -aes128, -aes192, -aes256
                 encrypt PEM output with cbc aes                        //使用aes加密输出的文件
 -camellia128, -camellia192, -camellia256
                 encrypt PEM output with cbc camellia                   //使用camellia加密输出的文件呢
 -text           print the key in text                                  //以明文形式输出各个参数值
 -noout          don not print key out                                    //不输出密钥到任何文件
 -modulus        print the RSA key modulus                              //输出模数指
 -check          verify key consistency                                 //检查输入密钥的正确性和一致性
 -pubin          expect a public key in input file                      //指定输入文件是公钥
 -pubout         output a public key                                    //指定输出文件是公钥
 -engine e       use engine e, possibly a hardware device.              //指定三方加密库或者硬
```

示例 

```bash
/*生成不加密的RSA密钥*/
$ openssl genrsa -out RSA.pem

/*为RSA密钥增加口令保护*/
$ openssl rsa -in RSA.pem -des3 -passout pass:123456 -out E_RSA.pem
writing RSA key

/*为RSA密钥去除口令保护*/
$ openssl rsa -in E_RSA.pem -passin pass:123456 -out P_RSA.pem
writing RSA key

/*比较原始后的RSA密钥和去除口令后的RSA密钥，是一样*/
$ diff RSA.pem P_RSA.pem

/*提取公钥，用pubout参数指定输出为公钥*/
openssl rsa -in E_RSA.pem -passin pass:123456 -pubout -out pub.pem
```

### rsautl指令说明

rsautl则是真正用于密钥交换和数字签名。实质上就是使用RSA公钥或者私钥加密。

```bash
$ openssl rsautl - 
Usage: rsautl [options]                  
-in file        input file                                           //输入文件
-out file       output file                                          //输出文件
-inkey file     input key                                            //输入的密钥
-keyform arg    private key format - default PEM                     //指定密钥格式
-pubin          input is an RSA public                               //指定输入的是RSA公钥
-certin         input is a certificate carrying an RSA public key    //指定输入的是证书文件
-ssl            use SSL v2 padding                                   //使用SSLv23的填充方式
-raw            use no padding                                       //不进行填充
-pkcs           use PKCS#1 v1.5 padding (default)                    //使用V1.5的填充方式
-oaep           use PKCS#1 OAEP                                      //使用OAEP的填充方式
-sign           sign with private key                                //使用私钥做签名
-verify         verify with public key                               //使用公钥认证签名
-encrypt        encrypt with public key                              //使用公钥加密
-decrypt        decrypt with private key                             //使用私钥解密
-hexdump        hex dump output                                      //以16进制dump输出
-engine e       use engine e, possibly a hardware device.            //指定三方库或者硬件设备
-passin arg    pass phrase source                                    //指定输入的密码
```

示例：

```bash
#使用rsautl进行加密和解密操作
$ openssl genrsa -des3 -passout pass:123456 -out RSA.pem 
Generating RSA private key, 512 bit long modulus
............++++++++++++
...++++++++++++
e is 65537 (0x10001)
/*提取公钥*/
$ openssl rsa -in RSA.pem -passin pass:123456 -pubout -out pub.pem 
writing RSA key
/*使用RSA作为密钥进行加密，实际上使用其中的公钥进行加密*/
$ openssl rsautl -encrypt -in plain.txt -inkey RSA.pem -passin pass:123456 -out enc.txt
/*使用RSA作为密钥进行解密，实际上使用其中的私钥进行解密*/
$ openssl rsautl -decrypt -in enc.txt -inkey RSA.pem -passin pass:123456 -out replain.txt
/*比较原始文件和解密后文件*/
$ diff plain.txt replain.txt 

/*使用公钥进行加密*/
$ openssl rsautl -encrypt -in plain.txt -inkey pub.pem -pubin -out enc1.txt
/*使用RSA作为密钥进行解密，实际上使用其中的私钥进行解密*/
$ openssl rsautl -decrypt -in enc1.txt -inkey RSA.pem -passin pass:123456 -out replain1.txt
/*比较原始文件和解密后文件*/
xlzh@cmos:~/test$ diff plain.txt replain1.txt


# 使用rsautl进行签名和验证操作
/*提取PCKS8格式的私钥*/
$ openssl pkcs8 -topk8 -in RSA.pem -passin pass:123456 -out pri.pem -nocrypt
/*使用RSA密钥进行签名，实际上使用私钥进行加密*/
$ openssl rsautl -sign -in plain.txt -inkey RSA.pem -passin pass:123456 -out sign.txt
/*使用RSA密钥进行验证，实际上使用公钥进行解密*/
$ openssl rsautl -verify -in sign.txt -inkey RSA.pem -passin pass:123456 -out replain.txt
/*对比原始文件和签名解密后的文件*/
$ diff plain.txt replain.txt 
/*使用私钥进行签名*/
$ openssl rsautl -sign -in plain.txt -inkey pri.pem  -out sign1.txt
/*使用公钥进行验证*/
$ openssl rsautl -verify -in sign1.txt -inkey pub.pem -pubin -out replain1.txt
/*对比原始文件和签名解密后的文件*/
$ cat plain replain1.txt
```

# SSL和SSH和OpenSSH，OpenSSL有什么区别

SSL（Secure Sockets Layer）是通讯链路的附加层。可以包含很多协议。https, ftps, .....;是一种国际标准的加密及身份认证通信协议，您用的浏览器就支持此协议。SSL协议使用通讯双方的客户证书以及CA根证书，允许客户/服务器应用以一种不能被偷听的方式通讯，在通讯双方间建立起了一条安全的、可信任的通讯通道。它具备以下基本特征：信息保密性、信息完整性、相互鉴定。 主要用于提高应用程序之间数据的安全系数。SSL协议的整个概念可以被总结为：一个保证任何安装了安全套接字的客户和服务器间事务安全的协议，它涉及所有TC/IP应用程序。另外还有一个额外的好处就是传输的数据是经过压缩的，所以可以加快传输的速度。

SSH（Secure SHell）只是加密的shell，最初是用来替代telnet的。通过port forward，也可以让其他协议通过ssh的隧道而起到加密的效果。SSH是由客户端和服务端的软件组成的，有两个不兼容的版本分别是：1.x和2.x。用SSH 2.x的客户程序是不能连接到SSH 1.x的服务程序上去的。OpenSSH 2.x同时支持SSH 1.x和2.x。

SSH提供两种级别的安全验证：

-  第一种级别（基于口令的安全验证）只要你知道自己帐号和口令，就可以登录到远程主机。所有传输的数据都会被加密，但是不能保证你正在连接的服务器就是你想连接的服务器。可能会有别的服务器在冒充真正的服务器，也就是受到“中间人”这种方式的攻击。

-  第二种级别（基于密匙的安全验证）需要依靠密匙，也就是你必须为自己创建一对密匙，并把公用密匙放在需要访问的服务器上。如果你要连接到SSH服务器上，客户端请求连接服务器，服务器将一个随机字符串发送给客户端，客户端根据自己的私钥加密这个随机字符串之后再发送给服务器，服务器接受到加密后的字符串之后用公钥解密，如果正确就让客户端登录，否则拒绝。但是，与第一种级别相比，第二种级别不需要在网络上传送口令。第二种级别不仅加密所有传送的数据，而且“中间人”这种攻击方式也是不可能的（因为他没有你的私人密匙）。但是整个登录的过程可能需要10秒。

OpenSSL：一个C语言函数库，是对SSL协议的实现。

OpenSSH：是对SSH协议的实现。

从编译依赖上看：

1. openssh依赖于openssl，没有openssl的话openssh就编译不过去，也运行不了。
2. HTTPS可以使用TLS或者SSL协议，而openssl是TLS、SSL协议的开源实现，提供开发库和命令行程序。openssl很优秀，所以很多涉及到数据加密、传输加密的地方都会使用openssl的库来做。
3. 可以理解成所有的HTTPS都使用了openssl。