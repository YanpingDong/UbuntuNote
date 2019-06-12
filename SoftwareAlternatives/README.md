# 通过软件中心安装到哪个目录

软件中心安装支持 deb和snap两种格式，所以运行命今目录会有不同，安装的软件一般都在/home 目录下面，目录名字是“.软件名”　，里面有配置文件;命令是在/usr目录里（具体命令路径可用```which or whereis```命令名查询）,如果安装的是snap应用启动命令则会在 /snap下

比如查看firefox在哪：
```
ps -ef | grep -i softname  会例出启动命令
tra 1885  1  4 10:02 tty2  00:03:01 /usr/lib/firefox/firefox -new-window
```
可以看到是在/usr/lib下的，所以可以确认是通过系统安装的。如果是自己通过下载源码编译并命令行安装的会到/usr/local/lib/中存放启动命令（我在自己安装python3.7的时候验证是该结果）

**NOTE:** 
- 如果是deb格式，那么即可以使用```dpkg -L softname```查看所有相关文件都安装在何处; 也可以使用```dpkg -l softname```查看版本信息 和 ```dpkg -S softname```查看包所拥有的文件;
- 还有一种程序是直接安装在/opt目录下的,该目录会存放程序所有相关文件。比如teamviewer

# nixnote2

**安装：**

- sudo add-apt-repository ppa:nixnote/nixnote2-stable
- sudo apt update
- apt install nixnote2 (在ubuntu 18版本下只需要这一个命令即可)

    Nixnote 是一个 Evernote 开源客户端，原名 Nevernote。Evernote 是一个著名的笔记等个人资料整理和同步软件， 因为 Evernote 没有 Linux 下的官方版本，因此出现了这个使用 Evernote 开放 API 实现的客户端，现在已经可以运行在 Mac、Linux 和 Windows 上。支持通过 Evernote 账户同步你的笔记等各种数据。

但用命令行安装的没有图标，要自己补图标，所以有人建议直接下载用dkpg安装并安装其依赖包即可，下载地址：https://sourceforge.net/projects/nevernote/

当然也可以通过Wine来安装Evernote，但可能会出现中文字体乱码。可以通Evernot的Tool-->Options-->Note下去选择支持中文的字体。我本机使用的是Noto Sans Mono CJK TC Regular字体

**添加账户与更新：**

File-->Add user完成帐户添加。然后点Sync完成授权与更新，授权的时候会弹出对话界面，输入完帐户如果没有弹出密码输入框，用鼠标点击一下右下角的create account就好

注：因为印象笔记分为国服和国际版，所以在创建账户的时候需要做选择如下图所示左为国际版本，右为国内版本。

![nixnote2_login](pic/nixnote2_login.png)

这个也可以使用RamBox做代理处理，但有的时候我们没有网络的时候也是需要使用的，所以还是用该客户端来使用以解决离线使用问题

# RamBox

免费的开源消息和电子邮件应用程序，可以将常见的Web应该程序组合在一起，即只要是有网页端的都可以使用RamBox做代理客户端来访问。比如，可以将WeChat的页面端交给RamBox来代理。这样我们不用在浏览器多个页面中来回切换找WeChat了。

安装
- Step1.到https://github.com/ramboxapp/community-edition/releases/下载
- Step2.如果是deb的用sudo dpkg -i xxx.deb
- Step3.在命令行输入rambox启动看一下是否正常

使用中可能出现的问题：输入法不能切换或者你点Rambox桌面图标没有办法启动。可以在命令行输入rambox看是否有错，我用的时候发现Rambox是以Root权限安装，导致我用用户权限时~	/.config/rambox这个目录没有权限，我用chmod 777强制更改了，以上两个问题就都解决了。或者用chown [-R]  用户名：用户组名 文件或目录 

# JDK安装

第一种方式下载JDK
- Step 1：
在https://www.oracle.com/technetwork/java/javase/downloads/index.html下选择下载版本，我本地是jdk-8u181-linux-x64.tar.gz。
- Step2: 用tar解压，或者直接在图形界面解压出来即可
- Step3: 编辑/etc/profile文件加入JAVA_HOME;PATH;CLASSPATH(可加可不加);其中JAVA_HOME是你的解压目录
```
# the following configuration is for jdk
export JAVA_HOME=/install_path/jkdx.x.x
epxort PATH=$PATH:$JAVA_HOME/bin
epxpor CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
```

- Step4：执行 chmod +x profile ，把profile变成可执行文件
- Step5：执行 source profile, 把profile里的内容执行生效
- Step6： 执行 java、 javac、java -version 查看是否安装成功.

第二种方式通过APT：
- Step1:sudo add-apt-repository ppa:webupd8team/java
- Step2: sudo apt-get update
- Step3: sudo apt-get install oracle-java8-installer


# Tcpdump

cpdump是Linux自带的抓包工具，可以详细看到计算机通信中详细报文内容

参数说明：

```
-i 指定要抓取数据包的网卡名称
-c 指定抓取包的个数
-w 把抓取到的数据存放到文件中供以后分析
```

示例：

```
$ tcpdump -i eth0 # 抓取eth0网卡的数据包 
$ tcpdump -i eth0 -c 10 # 只抓取10个包 
$ tcpdump -i eth0 -c 10 -w my-packets.pcap  
# file my-packets.pcap  
my-packets.pcap: tcpdump capture file .... 

#指定过滤端口(port)和主机名(host)
$ tcpdump -n -i eth0 port 80  
$ tcpdump -n -i eth0 host baidu.com  
```

# wine

在16.04下的安装过程如下

```bash
wget -nc https://dl.winehq.org/wine-builds/Release.key
sudo apt-key add Release.key
sudo apt-add-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ xenial main'
sudo apt-get update
sudo apt-get install --install-recommends winehq-stable
wine --version
```

wine的源地址：https://dl.winehq.org/wine-builds/

## exe文件的安装
使用命令：wine exe文件在Linux上的路径加文件名，例如：wine /home/user/download/tim.exe

## exe程序的卸载

**使用删除文件法：**

1.  wine会在/home下的用户名目录生成三个隐藏的文件夹 .wine、.local、.config 等文件夹，快捷键 ctrl+H 可以显示出来;
2.  进入 .wine 文件夹可以看到 drive_c 文件夹，这是wine自动生成的虚拟windows  C盘，里面有类似windows系统盘的目录结构，在里面找到需要卸载的软件文件夹删除即可；
3.  找到/home/用户名/.local/share/applications/wine/Programs，将软件对应的文件删除；
4.  找到/home/用户名/.config/menus/applications-merged，将软件对应的文件删除；
5.  这时候已经删除完毕，但是可能还会看到桌面图标或软件列表，重启系统即可。

**命令行**

wine uninstaller


## Wine的相关配置命令

winecfg： Wine配置编辑器,通过该命令会弹出一个Wine Configuration GUI，方便配置wine。比如模拟的windows系统版本等

winetricks:GUI版本，用来配置wine的windows环境

ntlm_auth >= 3.0.25Error解决方法
```bash
ERROR INFO:Make sure that ntlm_auth >= 3.0.25 is in your path. Usually, you can find it in the winbind package of your distribution.

$sudo apt install winbind
````

[字体下载](http://www.font5.com.cn),下载的字体直接放到```~/.wine/drive_c/windows/Fonts```下即可让wine支持该字体