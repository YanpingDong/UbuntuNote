# Linux下在一行执行多条命令

要实现在一行执行多条Linux命令，分三种情况：

1. &&

```
示例：
lpr /tmp/t2 && rm /tmp/t2
第2条命令只有在第1条命令成功执行之后才执行。当&&前的命令“lpr /tmp/t2”成功执行后"rm /tmp/t2"才执行，根据命令产生的退出码判断是否执行成功（0成功，非0失败）。
```

2. ||

```
示例：
cp /tmp/t2 /tmp/t2.bak || rm /tmp/t2
只有||前的命令“cp /tmp/t2 /tmp/t2.bak”执行不成功（产生了一个非0的退出码）时，才执行后面的命令。
```

3. ;

```
示例：
cp /tmp/t2 /tmp/t2.bak; echo "hello world"
顺序执行多条命令，当‘;’号前的命令执行完（不管是否执行成功）才执行‘;’后的命令。
```


# update-alternatives

**介绍**

我们可能会同时安装有很多功能类似的程序和可选配置，可能会出现同一软件的多个版本并存的场景。比如像是一些编程语言工具，一些系统中自带的是python2.6，而现在python2.7和python3.4使用较多，还有java有1.6，1.7和1.8版本。

update-alternatives是Debian系统中专门维护系统命令链接符的工具，通过它可以很方便的设置系统默认使用哪个命令、哪个软件版本，比如系统中同时安装了open jdk和sun jdk两个版本，而我们又希望系统默认使用sun jdk，通过update-alternatives就可以方便实现管理。

**格式参数**

命令格式：update-alternatives [<选项> ...] <命令> 

主要参数：

```
Commands:
  --install <link> <name> <path> <priority>
    [--slave <link> <name> <path>] ...
                           在系统中加入一组替换项.
  --remove <name> <path>   从 <名称> 替换组中去除 <路径> 项.
  --remove-all <name>      从替换系统中删除 <名称> 替换组.
  --auto <name>            将 <名称> 的主链接切换到自动模式.
  --display <name>         显示关于 <名称> 替换组的信息.
  --query <name>           machine parseable version of --display <name>.
  --list <name>            列出 <名称> 替换组中所有的可用替换项.
  --get-selections         list master alternative names and their status.
  --set-selections         read alternative status from standard input.
  --config <name>          列出 <名称> 替换组中的可选项，并就使用其中
                                 哪一个，征询用户的意见.
  --set <name> <path>      将 <路径> 设置为 <名称> 的替换项.
  --all                    对所有可选项一一调用 --config 命令.

<link> 是指向 /etc/alternatives/<名称> 的符号链接>.
  (e.g. /usr/bin/pager)
<name> 是该链接替换组的主控名.
  (e.g. pager)
<path> 是替换项目标文件的位置.
  (e.g. /usr/bin/less)
<priority> 是一个整数，在自动模式下，这个数字越高的选项，其优先级也就越高.

Options:
  --altdir <directory>     指定不同的可选项目录.
  --admindir <directory>   指定不同的管理目录.
  --log <file>             设置log文件.
  --force                  allow replacing files with alternative links.
  --skip-auto              skip prompt for alternatives correctly configured
                           in automatic mode (relevant for --config only)
  --verbose                详尽的操作进行信息，更多的输出.
  --quiet                  安静模式，输出尽可能少的信息.
  --help                   显示本帮助信息.
  --version                显示版本信息.
```

**示例**

系统如果默认的python3是3.5，现在我要将自己安装的python3.6设置为默认的python3命令的默认版本，执行下面的代码：

```bash
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 100
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 200
```

然后执行下面代码选择我们安装的python版本，即序列2：

```bash
 learlee@learleePC:/usr/bin$ update-alternatives --config python3 
There are 2 choices for the alternative python3 (providing /usr/bin/python3).

  Selection    Path                Priority   Status
------------------------------------------------------------
  0            /usr/bin/python3.6   2         auto mode
  1            /usr/bin/python3.5   1         manual mode
* 2            /usr/bin/python3.6   2         manual mode

Press <enter> to keep the current choice[*], or type selection number: 2
```

# netstat

**介绍**

Netstat 命令用于显示各种网络相关信息，如网络连接，路由表，接口状态 (Interface Statistics)，masquerade 连接，多播成员 (Multicast Memberships) 等等。

**格式参数**

命令格式 

```bash
netstat  [address_family_options]  [--tcp|-t]  [--udp|-u]  [--raw|-w]  [--listening|-l]  [--all|-a]  [--numeric|-n]
[--numeric-hosts]  [--numeric-ports]  [--numeric-users]  [--symbolic|-N]  [--extend|-e[--extend|-e]]  [--timers|-o]
[--program|-p] [--verbose|-v] [--continuous|-c]
```

主要参数：

```
    -r, --route              display routing table
    -i, --interfaces         display interface table
    -g, --groups             display multicast group memberships
    -s, --statistics         display networking statistics (like SNMP)
    -M, --masquerade         display masqueraded connections

    -v, --verbose            be verbose
    -W, --wide               don't truncate IP addresses
    -n, --numeric            don't resolve names
    --numeric-hosts          don't resolve host names
    --numeric-ports          don't resolve port names
    --numeric-users          don't resolve user names
    -N, --symbolic           resolve hardware names
    -e, --extend             display other/more information
    -p, --programs           display PID/Program name for sockets
    -c, --continuous         continuous listing

    -l, --listening          display listening server sockets
    -a, --all, --listening   display all sockets (default: connected)
    -o, --timers             display timers
    -F, --fib                display Forwarding Information Base (default)
    -C, --cache              display routing cache instead of FIB

  <Socket>={-t|--tcp} {-u|--udp} {-w|--raw} {-x|--unix} --ax25 --ipx --netrom
  <AF>=Use '-6|-4' or '-A <af>' or '--<af>'; default: inet
  List of possible address families (which support routing):
    inet (DARPA Internet) inet6 (IPv6) ax25 (AMPR AX.25) 
    netrom (AMPR NET/ROM) ipx (Novell IPX) ddp (Appletalk DDP) 
    x25 (CCITT X.25) 
```

***NOTE : LISTEN和LISTENING的状态只有用-a或者-l才能看到***

**状态说明**

    LISTEN：侦听来自远方的TCP端口的连接请求
    SYN-SENT：再发送连接请求后等待匹配的连接请求（如果有大量这样的状态包，检查是否中招了）
    SYN-RECEIVED：再收到和发送一个连接请求后等待对方对连接请求的确认（如有大量此状态，估计被flood攻击了）
    ESTABLISHED：代表一个打开的连接
    FIN-WAIT-1：等待远程TCP连接中断请求，或先前的连接中断请求的确认
    FIN-WAIT-2：从远程TCP等待连接中断请求
    CLOSE-WAIT：等待从本地用户发来的连接中断请求
    CLOSING：等待远程TCP对连接中断的确认
    LAST-ACK：等待原来的发向远程TCP的连接中断请求的确认（不是什么好东西，此项出现，检查是否被攻击）
    TIME-WAIT：等待足够的时间以确保远程TCP接收到连接中断请求的确认
    CLOSED：没有任何连接状态

**示例** 

```bash
netstat -nltp  #禁用反向域名解析功能，列出tcp监听端口并显示进程号
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:27206         0.0.0.0:*               LISTEN      11376/code      
tcp        0      0 127.0.1.1:53            0.0.0.0:*               LISTEN      -               
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -               
tcp        0      0 127.0.0.1:1080          0.0.0.0:*               LISTEN      31016/ss-qt5    
tcp6       0      0 ::1:631                 :::*                    LISTEN      


active 状态的套接字连接用 "ESTABLISHED" 字段表示，所以我们可以使用 grep 命令获得 active 状态的连接：
$ netstat -atnp | grep ESTA
(Not all processes could be identified, non-owned process info
will not be shown, you would have to be root to see it all.)
tcp 0 0 192.168.1.2:49156 173.255.230.5:80 ESTABLISHED 1691/chrome
tcp 0 0 192.168.1.2:33324 173.194.36.117:443 ESTABLISHED 1691/chrome


默认情况下 netstat 会通过反向域名解析技术查找每个 IP 地址对应的主机名。这会降低查找速度。如果你觉得 IP 地址已经足够，而没有必要知道主机名，就使用 -n 选项禁用域名解析功能。
netstat -ant  #列出所有tcp链接，禁用反向域名解析功能
```

# screenfetch

这个方便的 Bash 脚本可以用来生成那些漂亮的终端主题信息和用 ASCII 构成的发行版标志，就像如今你在别人的截屏里看到的那样。它会自动检测你的发行版并显示 ASCII 版的发行版标志，并且在右边显示一些有价值的信息。

安装方式：sudo apt-get install screenfetch

**示例**

```bash
screenfetch
                          ./+o+-       learlee@learleePC
                  yyyyy- -yyyyyy+      OS: Ubuntu 16.04 xenial
               ://+//////-yyyyyyo      Kernel: x86_64 Linux 4.15.0-47-generic
           .++ .:/++++++/-.+sss/`      Uptime: 7d 9h 0m
         .:++o:  /++++++++/:--:/-      Packages: 2066
        o:+o+:++.`..```.-/oo+++++/     Shell: bash 4.3.48
       .:+o:+o/.          `+sssoo+/    Resolution: 3840x1080
  .++/+:+oo+o:`             /sssooo.   DE: Unity 7.4.5
 /+++//+:`oo+o               /::--:.   WM: Compiz
 \+/+o+++`o++o               ++////.   WM Theme: MacBuntu-OS-X
  .++.o+++oo+:`             /dddhhh.   GTK Theme: MacBuntu-OS-X [GTK2/3]
       .+.o+oo:.          `oddhhhh+    Icon Theme: MacBuntu-OS
        \+.++o+o``-````.:ohdhhhhh+     Font: Apple Garamond 11
         `:o+++ `ohhhhhhhhyo++os:      CPU: Intel Core i5-4460 CPU @ 3.4GHz
           .o:`.syhhhhhhh/.oo++o`      GPU: AMD/ATI Caicos XT [Radeon HD 7470/8470 / R5 235/310 OEM]
               /osyyyyyyo++ooo+++/     RAM: 7526MiB / 15980MiB
                   ````` +oo+++o\:    
                          `oo++.   
```

# tee 

是在不影响原本 I/O 输出的情况下，将 stdout 复制一份到档案里

**格式参数**

命令格式：

tee [OPTION]... [FILE]...

参数说明：

```bash
-a, --append : append to the given FILEs, do not overwrite

-i, --ignore-interrupts : ignore interrupt signals
  
-p : diagnose errors writing to non pipes
      
--output-error[=MODE] : set behavior on write error.  See MODE below

--help : display this help and exit

--version : output version information and exit
```

**示例**

ls | tee test.txt,会在显示器中打出ls的结果，同时也会在test.txt里保存一份


# chown

chmod命令用来变更文件或目录的权限。在UNIX系统家族里，文件或目录权限的控制分别以读取、写入、执行3种一般权限来区分，另有3种特殊权限可供运用。用户可以使用chmod指令去变更文件与目录的权限，设置方式采用文字或数字代号皆可。符号连接的权限无法变更，如果用户对符号连接修改权限，其改变会作用在被连接的原始文件。

**格式参数**

命令格式：

chown [-R] 账号名称 文件或目录
chown [-R] 账号名称:用户组名称 文件或目录

参数：

```
-c或——changes：效果类似“-v”参数，但仅回报更改的部分；
-f或--quiet或——silent：不显示错误信息；
-R或——recursive：递归处理，将指令目录下的所有文件及子目录一并处理；常常用在更改某一目录的情况。
-v或——verbose：显示指令执行过程；
--reference=<参考文件或目录>：把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同；
<权限范围>+<权限设置>：开启权限范围的文件或目录的该选项权限设置；
<权限范围>-<权限设置>：关闭权限范围的文件或目录的该选项权限设置；
<权限范围>=<权限设置>：指定权限范围的文件或目录的该选项权限设置；

```

权限范围的表示法如下：

* u User，即文件或目录的拥有者；
* g Group，即文件或目录的所属群组；
* o Other，除了文件或目录拥有者或所属群组之外，其他用户皆属于这个范围；
* a All，即全部的用户，包含拥有者，所属群组以及其他用户；
* r 读取权限，数字代号为“4”;
* w 写入权限，数字代号为“2”；
* x 执行或切换权限，数字代号为“1”；
* \- 不具任何权限，数字代号为“0”；
* s 特殊功能说明：变更文件或目录的权限。

Linux用 户分为：拥有者、组群(Group)、其他（other），Linux系统中，预设的情況下，系统中所有的帐号与一般身份使用者，以及root的相关信息，都是记录在/etc/passwd文件中。每个人的密码则是记录在/etc/shadow文件下。 此外，所有的组群名称记录在/etc/group內！

linux文件的用户权限的分析图如下图所示
![pic/chmod.gif](pic/chmod.gif)


**示例**

```
chmod u+x,g+w f01　　//为文件f01设置自己可以执行，组员可以写入的权限
chmod u=rwx,g=rw,o=r f01 //和上示例相同

chmod 764 f01 //给拥有者rwx权限,给组rw权限,给其它用户r权限

chmod a+x f01　　//对文件f01的u,g,o都设置可执行属性

文件的属主和属组属性设置
chown user:market f01　　//把文件f01给uesr，添加到market组
```

# usermod

usermod命令用于修改用户帐号。用来修改用户帐号的各项设定。

**格式参数**

命令格式：usermod [options] LOGIN

参数：

```
-a|--append     ##把用户追加到某些组中，仅与-G选项一起使用 
-c|--comment    ##修改/etc/passwd文件第五段comment 
-d|--home       ##修改用户的家目录通常和-m选项一起使用 
-e|--expiredate ##指定用户帐号禁用的日期，格式YY-MM-DD 
-f|--inactive   ##用户密码过期多少天后采用就禁用该帐号，0表示密码已过期就禁用帐号，-1表示禁用此功能，默认值是-1 
-g|--gid        ##修改用户的gid，改组一定存在
-G|--groups     ##把用户追加到某些组中，仅与-a选项一起使用 
-l|--login      ##修改用户的登录名称 
-L|--lock       ##锁定用户的密码 
-m|--move-home  ##修改用户的家目录通常和-d选项一起使用 
-s|--shell      ##修改用户的shell 
-u|--uid        ##修改用户的uid，该uid必须唯一 
-U|--unlock     ##解锁用户的密码
```

**示例**

```bash
#1,新建用户test，密码test,另外添加usertest组
$ useradd test 
$ echo "test" | passwd --stdin test 
$ groupadd usertest 
#2,把test用户加入usertest组
$ usermod -aG usertest test ##多个组之间用空格隔开 
$ id test 
  uid=500(test) gid=500(test) groups=500(test),501(usertest) 
#3,修改test用户的家目录
$ usermod -md /home/usertest 
$ ls /home 
  usertest 
#4,修改用户名
$ usermod -l testnew(新用户名称)  test(原来用户名称) 
$ id testnew 
  uid=500(testnew) gid=500(test) groups=500(test),501(usertest) 
#5,锁定testnew的密码
$ sed -n '$p' /etc/shadow 
  testnew:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t 
  Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7::: 
$ usermod -L testnew 
$ sed -n '$p' /etc/shadow 
  testnew:!$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t 
  Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7::: 
#6,解锁testnew的密码
$ usermod -U testnew 
$ sed -n '$p' /etc/shadow 
  testnew:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t 
  Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7::: 
#7,修改用户的shell
$ sed '$!d' /etc/passwd 
  testnew:x:500:500::/home/usertest:/bin/bash 
$ usermod -s /bin/sh testnew 
$ sed -n '$p' /etc/passwd 
  testnew:x:500:500::/home/usertest:/bin/sh 
# 也可以手动编辑 vi /etc/passwd 找到testnew编辑保存即可
$ vi /etc/password

#8,修改用户的UID
$ usermod -u 578 testnew (UID必须唯一) 
$ id testnew 
  uid=578(testnew) gid=500(test) groups=500(test),501(usertest) 
#9,修改用户的GID
$ groupadd -g 578 test1 
$ usermod -g 578 testnew (578组一定要存在) 
$ id testnew 
  uid=578(testnew) gid=578(test1) groups=578(test1),501(usertest) 
#10,指定帐号过期日期
$ sed -n '$p' /etc/shadow 
  testnew:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t 
  Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7::: 
$ usermod -e 2012-09-11 testnew 
$ sed -n '$p' /etc/shadow 
  testnew:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t 
  Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7::15594: 
11,指定用户帐号密码过期多少天后，禁用该帐号
$ usermod -f 0 testnew 
$ sed -n '$p' /etc/shadow 
  testnew:$6$1PwPVBn5$o.MIEYONzURQPvn/YqSp69kt2CIASvXhOnjv/t 
  Z5m4NN6bJyLjCG7S6vmji/PFDfbyITdm1WmtV45CfHV5vux/:15594:0:99999:7:0:15594:
```

# lsof

在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。所以如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等，系统在后台都为该应用程序分配了一个文件描述符，无论这个文件的本质如何，该文件描述符为应用程序与基础操作系统之间的交互提供了通用接口。因为应用程序打开文件的描述符列表提供了大量关于这个应用程序本身的信息，因此通过lsof工具能够查看这个列表对系统监测以及排错将是很有帮助的。

**格式参数**

命令格式： lsof ［options］ filename

参数：

```
+d <目录>  列出目录下被打开的文件
+D <目录>  递归列出目录下被打开的文件
-a 列出打开文件存在的进程
-c <进程名> 列出指定进程所打开的文件
-g  列出GID号进程详情
-d <文件号> 列出占用该文件号的进程
-n <目录>  列出使用NFS的文件

-i <条件>  列出符合条件的进程。（4、6、协议、:端口、 @ip ）
  lsof -i [46] [protocol][@hostname|hostaddr][:service|port]
    46 --> IPv4 or IPv6
    protocol --> TCP or UDP
    hostname --> Internet host name
    hostaddr --> IPv4/IPv6地址
    service --> /etc/service中的 service name e.g., smtp (可以不止一个)
    port --> 端口号 (可以不止一个)

-p <进程号> 列出指定进程号所打开的文件

-u selects the listing of files for the user whose login names or user  ID  numbers  are  in  the  comma-separated set s - e.g.,``abe'', or ``548,root''.  (There should be no spaces  in  the set.)

-h 显示帮助信息
-v 显示版本信息
```

**示例**

```bash
lsof -u username  #列出某个用户打开的文件信息
lsof /filepath/file  #查看谁正在使用某个文件
lsof -c mysql  #列出某个程序所打开的文件信息
lsof -u test -c mysql  #列出某个用户以及某个程序所打开的文件信息
lsof -c -p 1234  #列出进程号为1234的进程所打开的文件
lsof -g gid  #显示归属gid的进程情况
lsof +d /usr/local/ #显示目录下被进程开启的文件
lsof +D /usr/local/# 同上，但是会搜索目录下的目录，时间较长
lsof -d 4 #显示使用fd为4的进程

```


# grep

（global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。 

**格式参数**

命令格式：
    grep [-abcEFGhHilLnqrsvVwxy][-A<显示列数>][-B<显示列数>][-C<显示列数>][-d<进行动作>][-e<范本样W式>][-f<范本文件>][--help][范本样式][文件或目录...]

参数：
```
-a 不要忽略二进制数据。
-A <显示列数> 除了显示符合范本样式的那一行之外，并显示该行之后的内容。
-b 在显示符合范本样式的那一行之外，并显示该行之前的内容。
-c 计算符合范本样式的列数。
-C <显示列数>或-<显示列数>  除了显示符合范本样式的那一列之外，并显示该列之前后的内容。
-d <进行动作> 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep命令将回报信息并停止动作。
-e<范本样式> 指定字符串作为查找文件内容的范本样式。
-E 将范本样式为延伸的普通表示法来使用，意味着使用能使用扩展正则表达式。
-f<范本文件> 指定范本文件，其内容有一个或多个范本样式，让grep查找符合范本条件的文件内容，格式为每一列的范本样式。
-F 将范本样式视为固定字符串的列表。
-G 将范本样式视为普通的表示法来使用。
-h 在显示符合范本样式的那一列之前，不标示该列所属的文件名称。
-H 在显示符合范本样式的那一列之前，标示该列的文件名称。
-i 忽略字符大小写的差别。
-l 列出文件内容符合指定的范本样式的文件名称。
-L 列出文件内容不符合指定的范本样式的文件名称。
-n 在显示符合范本样式的那一列之前，标示出该列的编号。
-q 不显示任何信息。
-R/-r 此参数的效果和指定“-d recurse”参数相同。
-s 不显示错误信息。
-v 反转查找。
-w 只显示全字符合的列。
-x 只显示全列符合的列。
-y 此参数效果跟“-i”相同。
-o 只输出文件中匹配到的部分。
```

**示例**

```
在文件中搜索一个单词，命令会返回一个包含“match_pattern”的文本行：
grep match_pattern file_name
grep "match_pattern" file_name

在多个文件中查找：
grep "match_pattern" file_1 file_2 file_3 ...

输出除之外的所有行 -v 选项：
grep -v "match_pattern" file_name

标记匹配颜色 --color=auto 选项：
grep "match_pattern" file_name --color=auto

使用正则表达式 -E 选项：
grep -E "[1-9]+"
或
egrep "[1-9]+"

只输出文件中匹配到的部分 -o 选项：
echo this is a test line. | grep -o -E "[a-z]+\."
line.
echo this is a test line. | egrep -o "[a-z]+\."
line.

统计文件或者文本中包含匹配字符串的行数 -c 选项：
grep -c "text" file_name

输出包含匹配字符串的行数 -n 选项：
grep "text" -n file_name
或
cat file_name | grep "text" -n

#多个文件
grep "text" -n file_1 file_2

在多级目录中对文本进行递归搜索：
grep "text" . -r -n
# .表示当前目录。

忽略匹配样式中的字符大小写：
echo "hello world" | grep -i "HELLO"
hello

选项 -e 制动多个匹配样式：
echo this is a text line | grep -e "is" -e "line" -o
is
line
#也可以使用-f选项来匹配多个样式，在样式文件中逐行写出需要匹配的字符。
cat patfile
aaa
bbb
echo aaa bbb ccc ddd eee | grep -f patfile -o

在grep搜索结果中包括或者排除指定文件：
#只在目录中所有的.php和.html文件中递归搜索字符"main()"
grep "main()" . -r --include *.{php,html}

#在搜索结果中排除所有README文件
grep "main()" . -r --exclude "README"

#在搜索结果中排除filelist文件列表里的文件
grep "main()" . -r --exclude-from filelist
```

# tail

用于查看文件的内容，有一个常用的参数 -f 常用于查阅正在改变的日志文件。

**格式参数**

命令格式：
tail [参数] [文件]  

参数：
```
-f 循环读取,tail -f filename 会把 filename 文件里的最尾部的内容显示在屏幕上，并且不断刷新，只要 filename 更新就可以看到最新的文件内容。
-q 不显示处理信息
-v 显示详细的处理信息
-c <数目> 显示的字节数
-n <行数> 显示行数
--pid=PID 与-f合用,表示在进程ID,PID死掉之后结束.
-q, --quiet, --silent 从不输出给出文件名的首部
-s, --sleep-interval=S 与-f合用,表示在每次反复的间隔休眠S秒 
```

**示例**

```bash
tail notes.log #要显示 notes.log 文件的最后 10 行

tail -f notes.log #跟踪名为 notes.log 的文件的增长情况此命令显示 notes.log 文件的最后 10 行。当将某些行添加至 notes.log 文件时，tail 命令会继续显示这些行。 显示一直继续，直到您按下（Ctrl-C）组合键停止显示。

tail +20 notes.log #显示文件 notes.log 的内容，从第 20 行至文件末尾

tail -c 10 notes.log  #显示文件 notes.log 的最后 10 个字符
``` 

# xargs

xargs 是给命令传递参数的一个过滤器，也是组合多个命令的一个工具。

- xargs 可以将管道或标准输入（stdin）数据转换成命令行参数，也能够从文件的输出中读取数据。
- xargs 也可以将单行或多行文本输入转换为其他格式，例如多行变单行，单行变多行。
- xargs 默认的命令是 echo，这意味着通过管道传递给 xargs 的输入将会包含换行和空白，不过通过 xargs 的处理，换行和空白将被空格取代。
- xargs 是一个强有力的命令，它能够捕获一个命令的输出，然后传递给另外一个命令。

之所以能用到这个命令，关键是由于很多命令不支持|管道来传递参数，而日常工作中有有这个必要，所以就有了 xargs 命令，例如：

find /sbin -perm +700 | ls -l       #这个命令是错误的
find /sbin -perm +700 | xargs ls -l   #这样才是正确的

*xargs 一般是和管道一起使用。*

**格式参数**

命令格式： Some_command | xargs -item  command

参数：

```bash
-a file #从文件中读入作为sdtin
-e flag #注意有的时候可能会是-E，flag必须是一个以空格分隔的标志，当xargs分析到含有flag这个标志的时候就停止。
-p  #当每次执行一个argument的时候询问一次用户。
-n num #后面加次数，表示命令在执行的时候一次用的argument的个数，默认是用所有的。
-t #表示先打印命令，然后再执行。
-i #或者是-I，这得看linux支持了，将xargs的每项名称，一般是一行一行赋值给 {}，可以用 {} 代替。
-r no-run-if-empty #当xargs的输入为空的时候则停止xargs，不用再去执行了。
-s num #命令行的最大字符数，指的是 xargs 后面那个命令的最大命令行字符数。
-L num #从标准输入一次读取 num 行送给 command 命令。
-l #同 -L。
-d delim #分隔符，默认的xargs分隔符是回车，argument的分隔符是空格，这里修改的是xargs的分隔符。
-x #exit的意思，主要是配合-s使用。。
-P #修改最大的进程数，默认是1，为0时候为as many as it can ，这个例子我没有想到，应该平时都用不到的吧。
```

**示例**
```
定义一个测试文件，内有多行文本数据：
# cat test.txt
a b c d e f g
h i j k l m n
o p q
r s t
u v w x y z

多行输入单行输出：

# cat test.txt | xargs
a b c d e f g h i j k l m n o p q r s t u v w x y z

-n 选项多行输出：
# cat test.txt | xargs -n3
a b c
d e f
g h i
j k l
m n o
p q r
s t u
v w x
y z

-d 选项可以自定义一个定界符：
# echo "nameXnameXnameXname" | xargs -dX
name name name name

结合 -n 选项使用：
# echo "nameXnameXnameXname" | xargs -dX -n2
name name
name name

～～～～～～～～～～～～～～～～～～～～～～～～～～～～~~
读取 stdin，将格式化后的参数传递给命令
假设一个命令为 sk.sh 和一个保存参数的文件 arg.txt：

sk.sh文件内容
=======================
#!/bin/bash
#sk.sh命令内容，打印出所有参数。

echo $*
=======================

arg.txt文件内容：
=======================
# cat arg.txt
aaa
bbb
ccc
=======================

使用 -I 指定一个替换字符串 {}，这个字符串在 xargs 扩展时会被替换掉，当 -I 与 xargs 结合使用，每一个参数命令都会被执行一次：
# cat arg.txt | xargs -I{} ./sk.sh -p {} -l
-p aaa -l
-p bbb -l
-p ccc -l
～～～～～～～～～～～～～～～～～～～～～～～～～～～～～～

复制所有图片文件到 /data/images 目录下：
ls *.jpg | xargs -n1 -I{} cp {} /data/images/

xargs 结合 find 使用
用 rm 删除太多的文件时候，可能得到一个错误信息：/bin/rm Argument list too long. 用 xargs 去避免这个问题：
find . -type f -name "*.log" -print0 | xargs -0 rm -f
xargs -0 将 \0 作为定界符。

统计一个源代码目录中所有 php 文件的行数：
find . -type f -name "*.php" -print0 | xargs -0 wc -l

查找所有的 jpg 文件，并且压缩它们：
find . -type f -name "*.jpg" -print | xargs tar -czvf images.tar.gz

xargs 其他应用
假如你有一个文件包含了很多你希望下载的 URL，你能够使用 xargs下载所有链接：
# cat url-list.txt | xargs wget -c
```


# curl

curl  is  a tool to transfer data from or to a server, using one of the supported protocols (DICT,FILE, FTP, FTPS, GOPHER, HTTP, HTTPS,  IMAP,IMAPS,LDAP,  LDAPS,  POP3,  POP3S,  RTMP, RTSP, SCP,SFTP,SMB, SMBS, SMTP, SMTPS, TELNET and TFTP)

**格式参数**

命令格式：curl [options...] <url>

参数：
```
-d, --data <data>   HTTP POST data
    --data-ascii <data> HTTP POST ASCII data
    --data-binary <data> HTTP POST binary data
    --data-raw <data> HTTP POST data, '@' allowed
    --data-urlencode <data> HTTP POST data url encoded
    --delegation <LEVEL> GSS-API delegation permission
    --digest        Use HTTP Digest Authentication
-i, --include       Include protocol response headers in the output
-I, --head          Show document info only
-P, --ftp-port <address> Use PORT instead of PASV
    --ftp-pret      Send PRET before PASV
    --ftp-skip-pasv-ip Skip the IP address for PASV
    --ftp-ssl-ccc   Send CCC after authenticating
    --ftp-ssl-ccc-mode <active/passive> Set CCC mode
    --ftp-ssl-control Require SSL/TLS for FTP login, clear for transfer
-G, --get           Put the post data in the URL and use GET
-H, --header <header/@file> Pass custom header(s) to server
-0, --http1.0       Use HTTP 1.0
    --http1.1       Use HTTP 1.1
    --http2         Use HTTP 2
    --http2-prior-knowledge Use HTTP 2 without HTTP/1.1 Upgrade
    --ignore-content-length Ignore the size of the remote resource
-4, --ipv4          Resolve names to IPv4 addresses
-6, --ipv6          Resolve names to IPv6 addresses
-x, --proxy [protocol://]host[:port] Use this proxy
-X, --request <command> Specify request command to use
    --request-target Specify the target for this request
    --resolve <host:port:address> Resolve the host+port to this address
    --retry <num>   Retry request if transient problems occur
    --retry-connrefused Retry on connection refused (use with --retry)
    --retry-delay <seconds> Wait time between retries
    --retry-max-time <seconds> Retry only within this period
    --sasl-ir       Enable initial response in SASL authentication
    --service-name <name> SPNEGO service name
-O, --remote-name   Write output to a file named as the remote file
-o, --output <file> Write to file instead of stdout
```

**示例**

```
POST请求
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ \ 
   "kingdom": "family", \ 
   "parameters": {} \ 
 }' 'http://host:port/closerTV/ott/top?userId=ziju&ver=3&clientId=1001'

GET 请求
发送数据时，不仅可以使用 POST 方式，也可以使用 GET 方式
curl -X GET http://www.example.com/api -d “somedata”
或者使用 -G 选项：
curl -G http://www.example.com/api -d “somedata” 

DELETE请求
curl -X DELETE https://api.github.cim

显示HTTP头和文件内容
curl -i http://www.codebelief.com

只显示 HTTP 头，而不显示文件内容
curl -I http://www.codebelief.com

将链接保存到文件
方法1： 我们可以使用 > 符号将输出重定向到本地文件中。
curl http://www.codebelief.com > index.html

方法2：也可以通过 curl 自带的 -o/-O 选项将内容保存到文件中。
-o（小写的 o）：结果会被保存到命令行中提供的文件名
-O（大写的 O）：URL 中的文件名会被用作保存输出的文件名
curl -o index.html http://www.codebelief.com
curl -O http://www.codebelief.com/page/2/
注意：使用 -O 选项时，必须确保链接末尾包含文件名，否则 curl 无法正确保存文件。如果遇到链接中无文件名的情况，应该使用 -o 选项手动指定文件名，或使用重定向符号。
```

# wc

该命令统计给定文件中的字节数、字数、行数。如果没有给出文件名，则从标准输入读取。wc同时也给出所有指定文件的总统计数。字是由空格字符区分开的最大字符串。 

**格式参数**

命令格式：
    wc [选项] 文件… 

参数：
```
- c 统计字节数。 
- l 统计行数。 
- w 统计字数。  
```

**示例**

```bash
~$ wc -lcw get-pip.py examples.desktop 
  22374   23043 1780465 get-pip.py
    240     569    8980 examples.desktop
  22614   23612 1789445 total
```

# command

command命令在shell脚本里面，如果发现有个函数和我们需要执行的命令同名，我们可以用command用来强制执行后面的命令，而不是同名函数，然后我们也可以在shell脚本里面判断莫个命令是否存在，我们平时一般用which命令也行。

**测试代码**

```bash
#!/bin/bash
function pwd()
{
    echo "I am pwd function"
}
  
echo "shell run pwd"
pwd
  
echo "shell command pwd"
command pwd
  
if  command -v pwd > /dev/null; then
    echo "pwd command has found"
else
    echo "pwd command has not found"
fi
  
if  command -v pwd1 > /dev/null; then
    echo "pwd1 command has found"
else
    echo "pwd1 command has not found"
fi
```
*NOTE: command -v pwd > /dev/null目的是把输出重定向到/dev/null中, -v是为了屏蔽掉command -v pwd1找不到会输出 pwd1: command not found类似错误*

输出：

```
shell run pwd
I am pwd function
shell command pwd
/home/chenyu/Desktop/linux/dabian/python
pwd command has found
pwd1 command has not found
```

# awk

awk是行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。

awk处理过程: 依次对每一行进行处理，然后输出;输入可以是文件也可以通过命令以管道型式输入

**格式参数**

命令格式： awk [-F|-f|-v] 'BEGIN{} //{command1; command2} END{}' file

主要由三部分组成：
- 参数：[-F|-f|-v] -F指定分隔符，-f调用脚本，-v定义变量 var=value
- Awk代码块：'  '引用代码块，这里有四部分，BEGIN{};//匹配;{}命令块;END{}结尾命令块
- 处理文件：file

说明：

- BEGIN 初始化代码块，在对每一行进行处理之前，初始化代码，主要是引用全局变量，设置FS分隔符
- // 匹配代码块，可以是字符串或正则表达式，例：/mysql/匹配mysql
- {} 命令代码块，包含一条或多条命令，例：{print $1; print $2},'；'为多条命令使用分号分隔
- END 结尾代码块，在对每一行进行处理之后再执行的代码块，主要是进行最终计算或输出结尾摘要信息

特殊要点:

```
$0   表示整个当前行
$1   每行第一个字段
NF   字段数量变量
NR   每行的记录号，多文件记录递增
FNR  与NR类似，不过多文件记录不递增，每个文件都从1开始
\t   制表符
\n   换行符
FS   BEGIN时定义分隔符
RS   输入的记录分隔符， 默认为换行符(即文本是按一行一行输入)
~    匹配，与==相比不是精确比较
!~   不匹配，不精确比较
==   等于，必须全部相等，精确比较
!=   不等于，精确比较
&&　 逻辑与
||   逻辑或
+    匹配时表示1个或1个以上
/[0-9][0-9]+/   两个或两个以上数字
/[0-9][0-9]*/   一个或一个以上数字
FILENAME  文件名
OFS  输出字段分隔符， 默认也是空格，可以改为制表符等
ORS  输出的记录分隔符，默认为换行符,即处理结果也是一行一行输出到屏幕
-F'[:#/]'  定义三个分隔符
print & $0
print 是awk打印指定内容的主要命令
awk '{print}'  /etc/passwd与awk '{print $0}'  /etc/passwd相同功能
```

**示例**

```
awk '{print " "}' /etc/passwd  #不输出passwd的内容，而是输出相同个数的空行，进一步解释了awk是一行一行处理文本
awk '{print "a"}'   /etc/passwd  #输出相同个数的a行，一行只有一个a字母
awk -F":" '{print $1}'  /etc/passwd  #以：号分隔每行数据并打出第1列
awk -F":" '{print $1; print $2}'   /etc/passwd   #将每一行的前二个字段，分行输出，进一步理解一行一行处理文本
awk  -F":" '{print $1,$3,$6}' OFS="\t" /etc/passwd  #输出字段1,3,6，以制表符作为分隔符
 
-f指定脚本文件
awk -f script.awk  file
BEGIN{
FS=":"
}
{print $1}   
#效果与awk -F":" '{print $1}'相同,只是分隔符使用FS在代码自身中指定

计算空行： 
awk 'BEGIN{X=0} /^$/{ X+=1 } END{print "I find",X,"blank lines."}' test 
I find 4 blank lines.

计算文件大小：
ls -l|awk 'BEGIN{sum=0} !/^d/ {sum+=$5} END{print "total size is",sum}'   
total size is 17487
 
-F指定分隔符
$1 指指定分隔符后，第一个字段，$3第三个字段， \t是制表符
一个或多个连续的空格或制表符看做一个定界符，即多个空格看做一个空格
awk -F":" '{print $1}'  /etc/passwd
awk -F":" '{print $1 $3}'  /etc/passwd  #$1与$3相连输出，不分隔
awk -F":" '{print $1,$3}'  /etc/passwd  #多了一个逗号，$1与$3使用空格分隔
awk -F":" '{print $1 " " $3}'  /etc/passwd  #$1与$3之间手动添加空格分隔
awk -F":" '{print "Username:" $1 "\t\t Uid:" $3 }' /etc/passwd   #自定义输出  
awk -F":" '{print NF}' /etc/passwd   #显示每行有多少字段
awk -F":" '{print $NF}' /etc/passwd   #将每行第NF个字段的值打印出来
awk -F":" 'NF==4 {print }' /etc/passwd   #显示只有4个字段的行
awk -F":" 'NF>2{print $0}' /etc/passwd   #显示每行字段数量大于2的行
awk '{print NR,$0}' /etc/passwd   #输出每行的行号
awk -F: '{print NR,NF,$NF,"\t",$0}' /etc/passwd  #依次打印行号，字段数，最后字段值，制表符，每行内容
awk -F: 'NR==5{print}' /etc/passwd   #显示第5行
awk -F: 'NR==5 || NR==6{print}'  /etc/passwd   #显示第5行和第6行
route -n|awk 'NR!=1{print}'  #不显示第一行
 
//匹配代码块
//纯字符匹配   !//纯字符不匹配   ~//字段值匹配    !~//字段值不匹配
awk '/mysql/' /etc/passwd
awk '/mysql/{print }' /etc/passwd
awk '/mysql/{print $0}' /etc/passwd               #以上三条指令结果一样
awk '!/mysql/{print $0}' /etc/passwd command/README.md#  #输出不匹配mysql的行command/README.md#
awk '/mysql|mail/{print}' /etc/passwdcommand/README.md#
awk '!/mysql|mail/{print}' /etc/passwcommand/README.md#d
awk -F":" '/mail/,/mysql/{print}' /etcommand/README.md#c/passwd     
command/README.md#
//区间匹配command/README.md#
awk '/[2][7][7]*/{print $0}' /etc/pascommand/README.md#swd   #匹配包含27为数字开头的行，如27，277，2777..command/README.md#.
awk -F":" '$1~/mail/{print $1}' /etc/command/README.md#passwd  #$1匹配指定内容才显示command/README.md#
awk -F: '{if($1~/mail/) print $1}' /ecommand/README.md#tc/passwd  #/与上面相同
awk -F: '$1!~/mail/{print $1}' /etc/passwd  #不匹配
awk -F: '$1!~/mail|mysql/{print $1}' /etc/passwd        
 
IF语句
必须用在{}中，且比较内容用()扩起来
awk -F: '{if($1~/mail/) print $1}' /etc/passwd  #简写
awk -F: '{if($1~/mail/) {print $1}}'  /etc/passwd  #全写
awk -F: '{if($1~/mail/) {print $1} else {print $2}}' /etc/passwd    #if...else...
 
 
条件表达式 （==   !=   >   >=）  
awk -F":" '$1=="mysql"{print $3}' /etc/passwd  
awk -F":" '{if($1=="mysql") print $3}' /etc/passwd    #与上面相同 
awk -F":" '$1!="mysql"{print $3}' /etc/passwd   #不等于
awk -F":" '$3>1000{print $3}' /etc/passwd   #大于
awk -F":" '$3>=100{print $3}' /etc/passwd   #大于等于
awk -F":" '$3<1{print $3}' /etc/passwd   #小于
awk -F":" '$3<=1{print $3}' /etc/passwd   #小于等于
 
逻辑运算符 （&&　||） 
awk -F: '$1~/mail/ && $3>8 {print }' /etc/passwd  #逻辑与，$1匹配mail，并且$3>8
awk -F: '{if($1~/mail/ && $3>8) print }' /etc/passwd
awk -F: '$1~/mail/ || $3>1000 {print }' /etc/passwd     #逻辑或
awk -F: '{if($1~/mail/ || $3>1000) print }' /etc/passwd 
 
数值运算
awk -F":" '$3 > 100' /etc/passwd    
awk -F":" '$3 > 100 || $3 < 5' /etc/passwd  
awk -F":" '$3+$4 > 200' /etc/passwd
awk -F":" '/mysql|mail/{print $3+10}' /etc/passwd  #第三个字段加10打印 
awk -F":" '/mysql/{print $3-$4}' /etc/passwd   #减法
awk -F":" '/mysql/{print $3*$4}' /etc/passwd   #求乘积
awk '/MemFree/{print $2/1024}' /proc/meminfo   #除法
awk '/MemFree/{print int($2/1024)}' /proc/meminfo #取整
 
输出分隔符OFS
awk '$6 ~ /FIN/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" netstat.txt
awk '$6 ~ /WAIT/ || NR==1 {print NR,$4,$5,$6}' OFS="\t" netstat.txt        
#输出字段6匹配WAIT的行，其中输出每行行号，字段4，5,6，并使用制表符分割字段
 
输出处理结果到文件
①在命令代码块中直接输出  route -n|awk 'NR!=1{print > "./fs"}'   
②使用重定向进行输出  route -n|awk 'NR!=1{print}'  > ./fs
 
格式化输出
netstat -anp|awk '{printf "%-8s %-8s %-10s\n",$1,$2,$3}' 
printf表示格式输出
%格式化输出分隔符
-8长度为8个字符
s表示字符串类型
打印每行前三个字段，指定第一个字段输出字符串类型(长度为8)，第二个字段输出字符串类型(长度为8),
第三个字段输出字符串类型(长度为10)
netstat -anp|awk '$6=="LISTEN" || NR==1 {printf "%-10s %-10s %-10s \n",$1,$2,$3}'
netstat -anp|awk '$6=="LISTEN" || NR==1 {printf "%-3s %-10s %-10s %-10s \n",NR,$1,$2,$3}'
 
IF语句
awk -F":" '{if($3>100) print "large"; else print "small"}' /etc/passwd
small
small
small
large
small
small

awk -F":" 'BEGIN{A=0;B=0} {if行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。($3>100) {A++; print "large"} else {B++; print "sm行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。all"}} END{print A,"\t",B}' /etc/passwd 行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。
#ID大于100,A加1，否则B加1行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。
行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。
awk -F":" '{if($3<100) next; 行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。else print}' /etc/passwd   #小于100跳过，否则显行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。示
awk -F":" 'BEGIN{i=1} {if(i<N行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。F) print NR,NF,i++ }' /etc/passwd   行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。
awk -F":" 'BEGIN{i=1} {if(i<N行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。F) {print NR,NF} i++ }' /etc/passwd行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。
另一种形式行处理器: 相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。
awk -F":" '{print ($3>100 ? "yes":"no")}'  /etc/passwd #第三列>100输出yes否则no
awk -F: '{print ($3>100 ? $3":\t yes":$3":\t no")}'  /etc/passwd #第三列>100输出第三列+制表符+yes否则输出第三列+制表符+no
 
while语句
awk -F: 'BEGIN{i=1} {while(i<NF) print NF,$i,i++}' /etc/passwd 
7 root 1
7 x 2
7 0 3
7 0 4
7 root 5
7 /root 6
 
数组
netstat -anp|awk 'NR!=1 {a[$6]++} END{for (i in a) print i,"\t",a[i]}'
netstat -anp|awk 'NR!=1 {a[$6]++} END{for (i in a) printf "%-20s %-10s %-5s \n", i,"\t",a[i]}'
9523                               1     
9929                               1     
LISTEN                            6     
7903                               1     
3038/cupsd                   1     
7913                               1     
10837                             1     
9833                               1     
 
应用1
awk -F":" '{print NF}' helloworld.sh   #输出文件每行有多少字段
awk -F: '{print $1,$2,$3,$4,$5}' helloworld.sh   #输出前5个字段(-F后的:号可以不用引号)
awk -F: '{print $1,$2,$3,$4,$5}' OFS='\t' helloworld.sh   #输出前5个字段并使用制表符分隔输出
awk -F: '{print NR,$1,$2,$3,$4,$5}' OFS='\t' helloworld.sh   #制表符分隔输出前5个字段，并打印行号,OFS不能移到代码块前
 
应用2
awk -F'[:#]' '{print NF}'  helloworld.sh   #指定多个分隔符: #，输出每行多少字段
awk -F'[:#]' '{print $1,$2,$3,$4,$5,$6,$7}' OFS='\t' helloworld.sh  #制表符分隔输出多字段,OFS设为制表符
 
应用3
awk -F'[:#/]' '{print NF}' helloworld.sh  #指定三个分隔符，并输出每行字段数
awk -F'[:#/]' '{print $1,$2,$3,$4,$5,$6,$7,$8,$9,$10,$11,$12}' helloworld.sh   #制表符分隔输出多字段
 
应用4
计算/home目录下，普通文件的大小，使用KB作为单位
ls -l|awk 'BEGIN{sum=0} !/^d/{sum+=$5} END{print "total size is:",sum/1024,"KB"}'
ls -l|awk 'BEGIN{sum=0} !/^d/{sum+=$5} END{print "total size is:",int(sum/1024),"KB"}'    #int是取整的意思
 
应用5
统计netstat -anp 状态为LISTEN和CONNECT的连接数量分别是多少
netstat -anp|awk '$6~/LISTEN|CONNECTED/ {sum[$6]++} END{for (i in sum) printf "%-10s %-6s %-3s \n", i," ",sum[i]}'
 
应用6
统计/home目录下不同用户的普通文件的总数是多少？
ls -l|awk 'NR!=1 && !/^d/{sum[$3]++} END{for (i in sum) printf "%-6s %-5s %-3s \n",i," ",sum[i]}'   
mysql        199 
root         374 

统计/home目录下不同用户的普通文件的大小总size是多少？
ls -l|awk 'NR!=1 && !/^d/ {sum[$3]+=$5} END{for (i in sum) printf "%-6s %-5s %-3s %-2s \n",i," ",sum[i]/1024/1024,"MB"}'
 
应用7
输出成绩表
awk 'BEGIN{math=0;eng=0;com=0;printf "Lineno.   Name    No.    Math   English   Computer    Total\n";printf "------------------------------------------------------------\n"}{math+=$3; eng+=$4; com+=$5;printf "%-8s %-7s %-7s %-7s %-9s %-10s %-7s \n",NR,$1,$2,$3,$4,$5,$3+$4+$5} END{printf "------------------------------------------------------------\n";printf "%-24s %-7s %-9s %-20s \n","Total:",math,eng,com;printf "%-24s %-7s %-9s %-20s \n","Avg:",math/NR,eng/NR,com/NR}' test0

[root@localhost home]# cat test0 
Marry   2143 78 84 77
Jack    2321 66 78 45
Tom     2122 48 77 71
Mike    2537 87 97 95
Bob     2415 40 57 62
```
 
awk手册
http://www.chinaunix.net/old_jh/7/16985.html


# sudo

以其它用户身份执行一个命令，其相关配置文件在/etc/sudoers文件中或在/etc/sudoers.d目录下的文件中，两者功能是一样的，只是在/etc/sudoers.d目录下的文件需要使用visudo -f /etc/sudoers.d/somefilename来做配置，而sudoers本身直接用visudo即可。

**为什么要使用/etc/sudoers.d/目录**

一般来讲/etc/sudoers是由版本发布管理者来控制的。假设你直接修改了/etc/sudoers文件，而这时管理者发布了新的版本，这时候你需要手动的检查更改并且使你的修改在新的版本中生效。但如果你把你的更改放到/etc/sudoers.d目录下的文件中的话，你就不必这样做，升级并不影响你之前的配置。但要注意，该目录下的文件名不能以’～’结尾，文件名不能包含’.’;且所有的文件应该是 0440（即-r--r----- ）
如何让/etc/sudoers.d中的配置文件生效
将/etc/sudoers文件中注掉的下行打开即可

```#includedir /etc/sudoers.d```

如何知道该用户可以执行哪些命令：sudo -l

## 如何配置sudoers文件

1.配置别名：这个在配置项很多且需要重复为多用户配置的时候很有用,别名规则定义格式如下：
Alias_Type NAME = item1, item2, ...
或Alias_Type NAME = item1, item2, item3 : NAME = item4, item5 

别名类型（Alias_Type）：别名类型包括如下四种 

- Host_Alias 定义主机别名； 
- User_Alias 用户别名，别名成员可以是用户，用户组（前面要加%号） 
- Runas_Alias 用来定义runas别名，这个别名指定的是“目的用户”，即sudo允许切换至的用户； 
- Cmnd_Alias 定义命令别名； 

NAME 就是别名了，NMAE的命名是包含大写字母、下划线以及数字，但**必须以一个大写字母开头**，比如SYNADM、SYN_ADM或SYNAD0是合法的，sYNAMDA或1SYNAD是不合法的；

**授权格式：**

授权用户 主机=[(切换到哪些用户：用户组)] [是否需要密码验证：] 命令1,[(切换到哪些用户：用户组)] [是否需要密码验证：] [命令2],[(切换到哪些用户：用户组)] [是否需要密码验证：] [命令3]...... 
凡是[ ]中的内容，是可以省略；命令与命令之间用,号分隔；通过本文的例子，可以对照着看哪些是省略了，哪些地方需要有空格； 
在[(切换到哪些用户：用户组)] ，如果省略，则默认为root用户；如果是ALL，则代表能切换到所有用户；注意要切换到的目的用户必须用()号括起来，比如(ALL)、(beinan) 

**一个完整的示例：**

假如我们就一台主机localhost，能通过hostname来查看，我们在这里就不定义主机别名了，用ALL来匹配所有可能出现的主机名；并且有 beinan、linuxsir、lanhaitun 用户；主要是通过小例子能更好理解；sudo虽然简单好用，但能把说的明白的确是件难事；最好的办法是多看例子和man soduers ； 

```
User_Alias SYSADER=beinan,linuxsir,%beinan 
User_Alias DISKADER=lanhaitun 
Runas_Alias OP=root 
Cmnd_Alias SYDCMD=/bin/chown,/bin/chmod,/usr/sbin/adduser,/usr/bin/passwd [A-Za-z]*,!/usr/bin/passwd root 
Cmnd_Alias DSKCMD=/sbin/parted,/sbin/fdisk 注：定义命令别名DSKCMD，下有成员parted和fdisk ； 
SYSADER ALL= SYDCMD,DSKCMD 
DISKADER ALL=(OP) DSKCMD 

注解： 
第一行：定义用户别名SYSADER 下有成员 beinan、linuxsir和beinan用户组下的成员，用户组前面必须加%号； 
第二行：定义用户别名 DISKADER ，成员有lanhaitun 
第三行：定义Runas用户，也就是目标用户的别名为OP，下有成员root 
第四行：定义SYSCMD命令别名，成员之间用,号分隔，最后的!/usr/bin/passwd root 表示不能通过passwd 来更改root密码； 
第五行：定义命令别名DSKCMD，下有成员parted和fdisk ； 
第六行： 表示授权SYSADER下的所有成员，在所有可能存在的主机名的主机下运行或禁止 SYDCMD和DSKCMD下定义的命令。更为明确的说， beinan、linuxsir和beinan用户组下的成员能以root身份运行 chown 、chmod 、adduser、passwd，但不能 更改root的密码；也可以以root身份运行 parted和fdisk ，本条规则的等价规则是； 

beinan,linuxsir,%beinan ALL= /bin/chown,/bin/chmod,/usr/sbin/adduser,/usr/bin/passwd [A-Za-z]*,!/usr/bin/passwd root,/sbin/parted,/sbin/fdisk

第七行：表示授权DISKADER 下的所有成员，能以OP的身份，来运行 DSKCMD ，不需要密码；更为明确的说 lanhaitun 能以root身份运行 parted和fdisk 命令；其等价规则是： 

lanhaitun ALL=(root) /sbin/parted,/sbin/fdisk 

可能有的弟兄会说我想不输入用户的密码就能切换到root并运行SYDCMD和DSKCMD 下的命令，那应该把把NOPASSWD:加在哪里为好？理解下面的例子吧，能明白的； 

SYSADER ALL= NOPASSWD: SYDCMD, NOPASSWD: DSKCMD 
```

# apt和Apt-get

**apt与apt-get之间的区别**

 通过 apt 命令，用户可以在同一地方集中得到所有必要的工具，apt 的主要目的是提供一种以「让终端用户满意」的方式来处理 Linux 软件包的有效方式。

apt 具有更精减但足够的命令选项，而且参数选项的组织方式更为有效。除此之外，它默认启用的几个特性对最终用户也非常有帮助。特性如下:

- 可以在使用 apt 命令安装或删除程序时看到进度条。

```
Progress: [  8%] [#####.....................................................] 
```
- apt 还会在更新存储库数据库时提示用户可升级的软件包个数。(52 packages can be upgraded)

```bash
learlee@learleePC:~$ sudo apt update
[sudo] password for learlee: 
Hit:1 http://mirrors.aliyun.com/ubuntu xenial InRelease
... ...
InRelease
Reading package lists... Done 
Building dependency tree       
Reading state information... Done
52 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

- 彩色字符支持等

|apt 命令| 	取代的命令 |	命令的功能|
| -------- | -----: | :---: | 
|apt install| 	apt-get install| 	安装软件包 |
|apt remove |	apt-get remove |	移除软件包 |
|apt purge |	apt-get purge  |	移除软件包及配置文件 |
|apt update |	apt-get update |	刷新存储库索引 |
|apt upgrade |	apt-get upgrade | 升级所有可升级的软件包 |
|apt autoremove |	apt-get autoremove |	自动删除不需要的包 |
|apt full-upgrade |	apt-get dist-upgrade |	在升级软件包时自动处理依赖关系 |
|apt search |	apt-cache search |	搜索应用程序 |
|apt show |	apt-cache show |显示装细节 |


apt 还有一些自己的命令(apt 命令也还在不断发展中)：

| apt 命令 | 命令的功能 |
| -------- | :---: | 
| apt list |	列出包含条件的包（已安装，可升级等）|
| apt edit-sources | 编辑源列表 (vim /etc/apt/sources.list)|

**apt-get已弃用?**

目前还没有任何 Linux 发行版官方放出 apt-get 将被停用的消息，至少它还有比 apt 更多、更细化的操作功能。对于低级操作，仍然需要 apt-get。

## 与命令相关的目录

**/var/lib/dpkg/available**

文件的内容是软件包的描述信息, 该软件包括当前系统所使用的 ubuntu 安装源中的所有软件包,其中包括当前系统中已安装的和未安装的软件包.

**/var/lib/dpkg/status**

安装过程中软件安装状态，个人认为```apt-get intsall -f```的修复即是通过该文件完成

**/var/lib/apt/lists**

使用```apt-get update```命令会从/etc/apt/sources.list中下载软件列表，并保存到该目录

**/var/cache/apt/archives**

目录是在用 ```apt-get install``` 安装软件时，软件包的临时存放路径

**/etc/apt/sources.list**

存放的是软件源站点入口信息
sources.list文件格式：
deb http://cn.archive.ubuntu.com/ubuntu/ precise main restricted

说明：
- 第一列：档案类型(Archive type) deb 或是 deb-src 表明了所获取的软件包档案类型。*deb:档案类型为二进制预编译软件包，一般我们所用的档案类型。deb-src:档案类型为用于编译二进制软件包的源代码。*
- 第二列：仓库地址 (Repository URL)，是软件包所在仓库的地址。我们可以更换仓库地址为其他地理位置更靠近自己的镜像来提高下载速度。

- 第三列：发行版 (Distribution)。发行版有两种分类方法，一类是发行版的具体代号，如 xenial,trusty, precise 等；还有一类则是发行版的发行类型，如oldstable, stable, testing 和 unstable。另外，在发行版后还可能有进一步的指定，如 xenial-updates, trusty-security, stable-backports 等。

- 第三列后：软件包分类 (Component)跟在发行版之后的就是软件包的具体分类了，可以有一个或多个。示例有两个，main和restricted。不同的 Linux 发行版对软件有着不同的分类，如：

```
Debian下：
- main：包含符合 DFSG 指导原则的自由软件包，而且这些软件包不依赖不符合该指导原则的软件包。这些软件包被视为 Debian 发型版的一部分。
- contrib：包含符合 DFSG 指导原则的自由软件包，不过这些软件包依赖不在 main 分类中的软件包。
- non-free：包含不符合 DFSG 指导原则的非自由软件包。
  
Ubuntu下：
- main：官方支持的自由软件。
- restricted：官方支持的非完全自由的软件。
- universe：社区维护的自由软件。
- multiverse：非自由软件。
```

**Note:如果要从sources.list的仓库地址找到发行版本，需要添加dists默认项，比如示例要看precise下有多少软件包分类的完整链接是http://cn.archive.ubuntu.com/ubuntu/dists/precise/ 从下图可以看到里面包含了main restricted两种软件包**

![软件包示例](pic/sources_sample.png)


## 工作原理：

Ubuntu采用集中式的软件仓库机制，将各式各样的软件包分门别类地存放在软件仓库中，进行有效地组织和管理。然后，将软件仓库置于许许多多的镜像服务器中，并保持基本一致。这样，所有的Ubuntu用户随时都能获得最新版本的安装软件包。因此，对于用户，这些镜像服务器就是他们的软件源（Reposity）。
然而，由于每位用户所处的网络环境不同，不可能随意地访问各镜像站点。为了能够有选择地访问，在Ubuntu系统中，使用软件源配置文件/etc/apt/sources.list列出最合适访问的镜像站点地址。


### apt-get update

程序分析/etc/apt/sources.list自动连网寻找list中对应的Packages/Sources/Release列表文件，如果有更新则下载之，存入/var/lib/apt/lists/目录
例：在sources.list有一个入口条目为：
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe那么在update后会在/var/lib/apt/lists中存入cn.archive.ubuntu.com_ubuntu_dists_bionic-updates_xxx相关文件，其中dists固定添加字段。示例如下：

```bash
learlee@learleePC:/var/lib/apt/lists$ ls | grep -i mirrors.aliyun.com_ubuntu_dists_xenial-security_universe_

mirrors.aliyun.com_ubuntu_dists_xenial-security_universe_binary-amd64_Packages
mirrors.aliyun.com_ubuntu_dists_xenial-security_universe_binary-i386_Packages
mirrors.aliyun.com_ubuntu_dists_xenial-security_universe_dep11_Components-amd64.yml.gz
mirrors.aliyun.com_ubuntu_dists_xenial-security_universe_dep11_icons-64x64.tar.gz
mirrors.aliyun.com_ubuntu_dists_xenial-security_universe_i18n_Translation-en

```

从上会发现其把url指定地址下所有的类型的数据包描述全部下载下来，每一个“_”符代表一级目录。以mirrors.aliyun.com_ubuntu_dists_xenial-security_universe_binary-amd64_Packages为例 ，其对应到URL地址为hhttp://mirrors.aliyun.com/ubuntu/dists/xenial-security/universe/binary-amd64/  在该地址下有两个Packages文件，一个是.gz，另一个是.xz

### apt-get install

apt-get install每次都会从/var/lib/apt/lists读取相应的文件，从而获取的Packages.gz包的信息。

Packages.gz中包含的信息有: 包名、优先级、类型、维护者、架构、源文件（source）、版本号、依赖包、冲突性信息、包大小、文件的下载路径、MD5sum、SHA1、包描述、Xul-Appid—应用程序id、Bugs信息、Origin、Supported等

/var/lib/apt/lists目录存储Packages文件中的一个包信息实例：

```
Package: accountwizard
Architecture: amd64
Version: 4:15.12.3-0ubuntu1.1
Priority: optional
Section: universe/utils
Source: kdepim
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Debian/Kubuntu Qt/KDE Maintainers <debian-qt-kde@lists.debi
an.org>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 2126
Depends: kdepim-runtime, libc6 (>= 2.14), libgcc1 (>= 1:3.0), libkf5akonadicore5
 (>= 15.07.90), libkf5akonadiwidgets5 (>= 15.07.90), libkf5codecs5 (>= 5.4.0+git
20141202.0008+15.04), libkf5configcore5 (>= 4.98.0), libkf5coreaddons5 (>= 4.100
.0), libkf5dbusaddons5 (>= 4.97.0), libkf5i18n5 (>= 5.0.0), libkf5identitymanage
ment5 (>= 15.07.90), libkf5itemviews5 (>= 4.96.0), libkf5kcmutils5 (>= 4.96.0), 
libkf5kiocore5 (>= 4.96.0), libkf5krosscore5 (>= 4.96.0), libkf5ldap5 (>= 15.07.
90), libkf5libkdepim5 (= 4:15.12.3-0ubuntu1.1), libkf5mailtransport5 (>= 15.08.1
), libkf5mime5 (>= 15.07.90), libkf5newstuff5 (>= 4.95.0), libkf5wallet-bin, lib
kf5wallet5 (>= 4.96.0), libkf5widgetsaddons5 (>= 4.98.0), libkf5xmlgui5 (>= 4.96
.0), libqt5core5a (>= 5.5.0), libqt5dbus5 (>= 5.0.2), libqt5gui5 (>= 5.0.2) | li
bqt5gui5-gles (>= 5.0.2), libqt5widgets5 (>= 5.3.0), libqt5xml5 (>= 5.0.2), libs
tdc++6 (>= 4.1.1)
Breaks: kdepim-runtime (<< 4:15.07.90~)
Replaces: kdepim-runtime (<< 4:15.07.90~)
Filename: pool/universe/k/kdepim/accountwizard_15.12.3-0ubuntu1.1_amd64.deb
Size: 357630
MD5sum: 55f5895b2b37b583eed6d417edb47961
SHA1: c0ee80464848c3b8798090f845983a3dc90645d3
SHA256: a4bba5c03df2e2b459436f69fcd38dc0a93c5209f297d48249d9fdf2788be05d
Homepage: http://pim.kde.org/
Description: wizard for KDE PIM applications account setup
Task: kubuntu-desktop, kubuntu-full
Description-md5: cb40f9c1dd4e29fdc2afadecae895725
Supported: 3y
```

- 从/etc/apt/sources.list中的配置```deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe``` 第二项再加上上述中Filename的值可以直接得到下载链接 ```http://mirrors.aliyun.com/ubuntu/pool/universe/k/kdepim/accountwizard_15.12.3-0ubuntu1.1_amd64.deb```

- 从Depends项可以看到该包依赖关系

使用“apt-get install”下载软件包大体分为4步：

- 第一步： 扫描本地存放的软件包更新列表（由“apt-get update”命令刷新更新列表，也就是/var/lib/apt/lists/），找到软件包；
- 第二步： 进行软件包依赖关系检查，找到支持该软件正常运行的所有软件包；
- 第三步： 从软件源所指 的镜像站点中，下载相关软件包；
- 第四步： 解压软件包，并自动完成应用程序的安装和配置。

### apt-get upgrade

使用“apt-get install”命令能够安装或更新指定的软件包。而在Ubuntu Linux中，只需一条命令就可以轻松地将系统中的所有软件包一次性升级到最新版本，这个命令就是“apt-get upgrade”，它可以很方便的完成在相同版本号的发行版中更新软件包。在依赖关系检查后，命令列出了目前所有需要升级的软件包，在得到用户确认后，便开始更新软件包的下载和安装。当然，apt- get upgrade命令会在最后以合理的次序，安装本次更新的软件包。系统更新需要用户等待一段时间。

### apt-cache pkgnames

查询以指定名字开头的安装包，比如:

```
tra@tra:~$ apt-cache pkgnames shadows
shadowsocks-libev
Shadowsocks
```

### apt-cache search
查询名字或描述中包含指定关键字的安装包，比如：

```
tra@tra:~$ apt-cache search shadowsocks
libshadowsocks-libev-dev - lightweight and secure socks5 proxy (development files)
libshadowsocks-libev2 - lightweight and secure socks5 proxy (shared library)
shadowsocks - Fast tunnel proxy that helps you bypass firewalls
shadowsocks-libev - lightweight and secure socks5 proxy
simple-obfs - simple obfusacting plugin for shadowsocks-libev
```