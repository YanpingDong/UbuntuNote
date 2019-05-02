
　　本篇记录只想告诉大家，如果哪个系统（当然这主要看该系统所附带的工具/软件是否能让你更好更高效的工作）能在你的工作或者生活环境带来效率就使用哪种系统，而不必纠结使用什么样的系统！！！

　　大家可以说我并不是一个纯粹的OS主义者，当然这点我承认，从崇拜Linux，仰望能自由使用Linux的"大神"们，到目前的“功利”主义思想的转变（当然我仍然很崇拜和仰望着你们），大家可以说是因为“能力”不足，导致没有办法用好linux，这并不是linux的错，如果硬要这样说，我也是接受的。但我想说的是任何一个工具的产生本质都是要提高“普通人”的生产效率的，让人们更加的专注高效的做“自己的工作”而非受更多外力的影响，社会分工不就是为了让每个人在各自的领域更加专注吗？一个系统如果真心要走向大众就要更接近大众的平均入门门槛线，要不然只能高高在上受人膜拜好了，不要谈普及率，更不要谈如何打败其它OS。

　　对于大部分人来说下面几点才是所观注重点：

1. 操作系统好上手, 你的界面友善
2. 你的操作系统稳定
3. 在你平台上的软件多, 便宜, 容易获得和安装

　　我本人目前在工作和生活当中仍然使用的是linux系统（ubuntu16 18两个版本）,写本篇记录也是为了记录我在使用过程中要掌握的linux的命令，要了解哪些部分的一个汇总，并不是一篇学习Linux的文章，因为没有系统性，内容很散，所以本文勉强可以当做一个使用参考（可能连使用参考都不能算，因为我们的使用范围可能差很远）。你可能要学会或了解哪些命令，本文中的替代软件是否能涵盖你目前使用的，而这些替代软件都有哪问题这些问题你是否能接受，看到这些大家可以做一个简单的评估该系统是否适合大家。

　　目前来说各种Linux桌面版本已经很多，相较前些年好多了，安装上随意找一个教程照着做有DIY能力的人应该都可以做到，但对于连Windows系统都需要其他人安装的，就不用想了。

　　为了方便大家参考，我先描述我的背景，平时生活我可能只看一下漫画浏览一下网页;听听音乐;看看视频等。工作主要从事的是后台服务开发，有的时候会涉及前端页面的开发。所以如果大家需求相同那么本文就很适合你了。另外我想和我家的宝宝以后玩编程机器人很多平台都是Linux的，所以工作和生活都没有办法避免。所以先用着让自己不再惧怕。

　　游戏玩家，重度Office用户可以直飘过了！！！

** 我只是希望去使用Linux系统，让其为我工作，并不是去探索Linux系统，所以深度大家就不要较真了 **


# 利与弊

　　单独出来写是期望大家能在第一时间看到，先浏览一遍即可以知道可能遇到的问题。方便大家考虑是否还需要继续阅读下去。这样不浪费大家时间。

　　没有在该部分出现的软件是因为我使用过程中一切顺利，所以并不需要在此特别指出。

## 利

　　这部分主要记录Linux系统对我有帮助的点，给我节省了哪些时间开销。

1. 我省了很多软件去链接Linux服务器，只需要使用ssh即可。以前在windows下我要安装XShell。

2. 上传下载Linux服务器文件以前要用putty,目前只需要使用scp。

3. 我主要从事后台开发，最终的服务需要部署到linux下，最明显的是目录不一致问题得到了解决（win:D:\a\b; Linux:/a/b），减少了部署的时候因开发和部署环境的不一致而导致的奇怪现像。

4. Python不再需要安装，2.X  3.X自带，测试Hadoop，tensorflow等的时候不再需要搭建虚拟机。

5. 用RamBox可以集合很多网页端应用，比如WeChat,Evernote,Youdao页面版等。当然这些应用都可以在浏览器里查看，但我们在找的时候会有问题，会在一大堆打开的页面里，可能一不小心就给整体关掉了，另一种就是开两个浏览器，即两个不同的浏览器，一个用来长开的应用页面，一个做平时页面浏览。但RamBox对浏览器来说会有提示框显示有消息。

```
个人认为RamBox（或双浏览器形式）更适合需要长期打开且必需要在联网的下使用的应用，比如WeChat。但如果你需要在离线的时候也需要使用，那么就很不适合了。比如evernote也有网页版，你没有一个离线客户端你在没有网络的时候就没有办法看到你以前的记录。所以虽然也能用但很受限。
```


## 弊

　　这部分主要记录我在使用过程中的种不顺，可能是因为我使用不当造成，如果有还请提示告知，我好更正。

1. Office我用过LibreOffice,WPS做替代，我只能说要求不高的话可以使用，如果要求很多，大部分工作都要在Office下工作的，可以不用往下看了，直接抛弃该系统吧，真心没有一个能超越Windows Office的。LibreOffice,WPS的好处我能想到的只有免费了。没有版权问题。WPS让人最不爽的是，我从FireFox下想拷贝一些页面上的文字的时候，不能直接贴入WPS我需要先贴在Linux的文本编辑器里，然后从编辑器里Copy到WPS。别外也有字体大小不一，乱码的问题。

2. 软件绘图draw.io，能用目前还没有遇到问题，但是导入Linux的office替代品中的时候需要用做图软件先保存成图片形式，这比windows下多了一步，windows下的visio数据是直接可以copy到world里的。

3. Electronic Wechat替代Wechat，这个实质上是一个网页版本，我使用它因为可以独立显是一个Icon这样我不用在浏览器Icon下找哪个页面是Wechat。但问题是这不copy数据，即你不能把聊天数据copy出来当文本使用。欲哭无泪啊～～有的时候对方发一个文本过来，你想做一个保存，你只能自己手动敲一遍。还有目前的2.0.1版本下显示扩展表情包有问题。

4. 页面版本Wechat，这个是很好用，但在你不使用截屏功能的前题下。另一个问题就是，你开了多个页面的时候如何快速的找出来聊天界面的问题了。最后就是没有聊天记录，你重新再开什么都没有（目前转为RamBox使用Wechat和网页版体验一至）

5. 我是有道词典的重度使用用户，目前我并没有找到一个比有道更无脑的使用方式的软件，你只需要安装，并联网，然后开始使用吧。所以我尝试用过网页版本，但不能发声！另一个问题和页面版Wechat一样，无法快速从多个页面第一时间找到他。（目前是在RamBox里使用baidu翻译，如果非要用有道我会转到虚拟机下开Win，目前只在离线的时候会切换到Win下因为youdao有离线包）

6. NixNote2（2.0.2-2build1）这个是用来替代EverNote客户端的一个开源客户端，目前出现的问题也是从网页Copy的时候有的不能直接被贴入。其它功能一切正常。



**以下文档中目录要看细节可以进文档中查看**
# [使用到的命令](command/README.md)

个人在工作和生活中使用到的命令，没有使用到的并不会在这里面出现，从中可以看出一般我个人需要掌握多少个命令，当然每个命令的使用频率并不会在本文档记录。如果要找命令的使用可以使用该[链接](http://www.runoob.com/linux/linux-command-manual.html.)。


## [update-alternative](command/README.md#update-alternative)
update-alternatives是Debian系统中专门维护系统命令链接符的工具，通过它可以很方便的设置系统默认使用哪个命令、哪个软件版本

## [netstat](command/README.md#netstat)
用于显示各种网络相关信息，如网络连接，路由表，接口状态等

## [screenfetch](command/README.md#screenfetch)
自动检测你的发行版并显示 ASCII 版的发行版标志，并且在右边显示OS,Kernel,CPU,GPU,RAM等信息

## [tee](command/README.md#tee)
是在不影响原本 I/O 输出的情况下，将 stdout 复制一份到档案里

## [chmod](command/README.md#chmod)
变更文件的拥有者和所属组

## [lsof](command/README.md#lsof)
lsof(list open files)是一个列出当前系统打开文件的工具
 
## [grep](command/README.md#grep)
它能使用正则表达式搜索文本，并把匹配的行打印出来。 

## [tail](command/README.md#tail)
用于从文件尾部查看文件的内容

## [xargs](command/README.md#xargs)
给命令传递参数的一个过滤器，也是组合多个命令的一个工具。

## [curl](command/README.md#curl)
cURL是一个利用URL语法在命令行下工作的文件传输工具，1997年首次发行。它支持文件上传和下载，所以是综合传输工具。

## [awk](command/README.md#awk)
行处理器,相比较屏幕处理的优点，在处理庞大文件时不会出现内存溢出或是处理缓慢的问题，通常用来格式化文本信息。逐行处理。

## [sudo](command/README.md#sudo)
以其它用户身份执行一个命令，其相关配置文件在/etc/sudoers文件中或在/etc/sudoers.d目录下的文件中，两者功能是一样的，只是在/etc/sudoers.d目录下的文件需要使用visudo -f /etc/sudoers.d/somefilename来做配置，而sudoers本身直接用visudo即可。

# 概念解释

# 输入法
# IDE

# 虚拟机
## 安装
## 设置共享文件夹

# VPS代理客户端安装
## shadowsocks界面版本
## shadowsocks命令行版本安装
## Http Https代理
## 浏览器设置代理（FireFox）

# PPA是什么

# 常用命令说明
## apt-get update
## apt-get install
## apt-get upgrade
## apt-cache pkgnames
## apt-cache search
## Apt-get apt区别
## netstat
## lsof
## screenfetch
## tee
## chown
## grep
## wc
## tail
## xargs
## curl

# 系统升级到新版本
# exec source 区别
# nautilus图形与终端的结合
# Git命令行分支显示
# 将 .rpm 文件转为 .deb 文件
# 显示隐藏文件
# 安装 tarballs

# 常用目录文件说明
## /etc/enviroment
## /etc/profile
## /etc/bashrc(不一定存在)
## ~/.bash_profile(不一定存在)
## ~/.bashrc
## ~/.bash_logout
## ~/.profile
## /usr/share/applications/
## 桌面快捷方式创建
## /dev/null文件
## /temp目录
## ~/.ssh目录

# 常用文件说明
## ssh config文件

# Linux标准文件描述符
# 管道和重定向之间的区别

# 常用软件替代方案
## Snap和Deb packages
## Snap常用命令
### cannot find signatures with metadata for snap
## 通过软件中心安装到哪个目录
## 截屏软件 Shutter
## 对比软件 meld
## 文本编辑软件atom
## 邮件客户端 thunderbird
### 中文内容乱码问题
## WeChat（RamBox替换）
## RamBox 网页版整合程序
## Office替代软件
## FireFox模拟IE
## Evernote客户端nixnote2
## Eclipse
## JDK安装

# 遇见问题
## Could not get lock /var/lib/dpkg/lock
## CLI中文乱码
## Ubuntu下耳机电流声消除方法
