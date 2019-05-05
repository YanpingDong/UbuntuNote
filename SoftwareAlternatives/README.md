# 常用软件替代方案

## 通过软件中心安装到哪个目录

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
