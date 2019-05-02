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
awk '!/mysql/{print $0}' /etc/passwd   #输出不匹配mysql的行
awk '/mysql|mail/{print}' /etc/passwd
awk '!/mysql|mail/{print}' /etc/passwd
awk -F":" '/mail/,/mysql/{print}' /etc/passwd     

//区间匹配
awk '/[2][7][7]*/{print $0}' /etc/passwd   #匹配包含27为数字开头的行，如27，277，2777...
awk -F":" '$1~/mail/{print $1}' /etc/passwd  #$1匹配指定内容才显示
awk -F: '{if($1~/mail/) print $1}' /etc/passwd  #/与上面相同
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

awk -F":" 'BEGIN{A=0;B=0} {if($3>100) {A++; print "large"} else {B++; print "small"}} END{print A,"\t",B}' /etc/passwd 
#ID大于100,A加1，否则B加1

awk -F":" '{if($3<100) next; else print}' /etc/passwd   #小于100跳过，否则显示
awk -F":" 'BEGIN{i=1} {if(i<NF) print NR,NF,i++ }' /etc/passwd   
awk -F":" 'BEGIN{i=1} {if(i<NF) {print NR,NF} i++ }' /etc/passwd
另一种形式
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

 
awk手册
http://www.chinaunix.net/old_jh/7/16985.html
    