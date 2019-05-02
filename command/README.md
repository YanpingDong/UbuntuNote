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

tail 命令可用于查看文件的内容，有一个常用的参数 -f 常用于查阅正在改变的日志文件。

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