# 清理磁盘占用空间

- Step1：查看磁盘占用率 ```df -h```
- Step2：进入根目录```cd /```，执行```du -sm * | sort -n```（磁盘占用的升序排列） 或```du -h --max-depth=1```
- Step3：进入占用空间较大的目录，继续执行```du -sm * | sort -n```，删除不重要的文件(rm命令)

可能遇到的删除文件之后空间没有释放,极有可能是文件被占用所以没有释放空间。lsof -n|grep deleted 查找占用文件的应用。然后杀掉（kill -9 pid）对应进程让系统清掉数据。

```
du : 显示每个文件和目录的磁盘使用空间~~~文件的大小。
命令参数：
-a  #显示目录中文件的大小  单位 KB 。
-b  #显示目录中文件的大小，以字节byte为单位。
-c  #显示目录中文件的大小，同时也显示总和；单位KB。
-k 、 -m  、#显示目录中文件的大小，-k 单位KB，-m 单位MB.
-s  #仅显示目录的总值，单位KB。
-h  #以K  M  G为单位显示，提高可读性~~~（最常用的一个~也可能只用这一个就满足需求了）


df：显示磁盘分区上可以使用的磁盘空间
这里只记住两个参数就好：
-a  #查看全部文件系统，单位默认KB
-h  #使用-h选项以KB、MB、GB的单位来显示，可读性高~~~（最常用）
```

# linux程序被Killed，如何精准查看日志

- Step1: cd /var/log/

- Step2: dmesg | egrep -i -B100 'killed process' 或: egrep -i -r 'killed process' /var/log 或: journalctl -xb | egrep -i 'killed process'

**dmesg文件**过滤后输出如下：

Killed process 11935 (python3) total-vm:2601976kB, anon-rss:652292kB, file-rss:0kB, shmem-rss:0kB

参数说明：      
- total-vm：进程总共使用的虚拟内存； 
- anon-rss：虚拟内存实际占用的物理内存； 
- file-rss：虚拟内存实际占用的磁盘空间； 

```
OOM killer感念说明：

LINUX内核Out-Of-Memory killer机制是一种防止内存耗尽影响系统运行而采用的一种自我保护机制。
根据内核源码oom_kill.c中的定义，系统会依据“进程占用的内存”，“进程运行的时间”，“进程的优先级”，“是否为 root 用户进程“，”子进程个数和占用内存“，”用户控制参数oom_adj ”等计算一个oom_score值，分数越高就越会被内核优先杀掉。
```

# CouldNotGetLock

当使用apt-get的时候如果遇到如下问题：

```bash
E: Could not get lock /var/lib/dpkg/lock - open (11 Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/) is another process using it? 
```

处理方案：

- Step 1：sudo lsof /var/lib/dpkg/lock 查找拥有该锁的进程，如查到不为空可以使用sudo kill -9 <PID> 
- Step 2：如果上一步是为空sudo rm /var/lib/dpkg/lock

```
参考链接：

https://blog.csdn.net/n66040927/article/details/81017658
http://www.mikewootc.com/wiki/linux/usage/ubuntu_service_usage.html
```