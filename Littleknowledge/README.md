# Linux 服务管理两种方式service和systemctl

两个都可以管理系统上运行的服务，比如启停等。

## service

service命令其实是去/etc/init.d目录下，去执行相关程序

```bash
# service命令启动redis脚本
service redis start
# 直接启动redis脚本
/etc/init.d/redis start
# 开机自启动 
update-rc.d redis defaults

# 以上三个命令在systemctl中分别对应start enable
```

**阅读service脚本**

结论：通过阅读service（/usr/sbin/service  ubuntu位置）自身脚本脚本可以知道其实是通过is_systemd变量来控制是否是用SysV的方式调用脚本还是通过systemctl来调用。如果是sysv方式则会从/etc/init.d下对应的脚本来运行。

如何选择sysv还是systemctl看下面脚本可以得出通过/run/systemd/system目录是否存在来设置is_systemd的值。

```bash
if [ -d /run/systemd/system ]; then
   is_systemd=1
fi
```

如果is_systemd的字串长不为0则使用systemctl来调用,简化后代码如下：

```bash
# When this machine is running systemd, standard service calls are turned into
# systemctl calls.
if [ -n "$is_systemd" ]
then
   UNIT="${SERVICE%.sh}.service"
   # avoid deadlocks during bootup and shutdown from units/hooks
   # which call "invoke-rc.d service reload" and similar, since
   # the synchronous wait plus systemd's normal behaviour of
   # transactionally processing all dependencies first easily
   # causes dependency loops
   if ! systemctl --quiet is-active multi-user.target; then
       sctl_args="--job-mode=ignore-dependencies"
   fi

   case "${ACTION}" in
      restart|status)
         exec systemctl $sctl_args ${ACTION} ${UNIT}
      ;;
      start|stop)
         # Follow the principle of least surprise for SysV people:
         # When running "service foo stop" and foo happens to be a service that
         # has one or more .socket files, we also stop the .socket units.
         # Users who need more control will use systemctl directly.
         for unit in $(systemctl list-unit-files --full --type=socket 2>/dev/null | sed -ne 's/\.socket\s*[a-z]*\s*$/.socket/p'); do
             if [ "$(systemctl -p Triggers show $unit)" = "Triggers=${UNIT}" ]; then
                systemctl $sctl_args ${ACTION} $unit
             fi
         done
         exec systemctl $sctl_args ${ACTION} ${UNIT}
      ;;
      reload)
         _canreload="$(systemctl -p CanReload show ${UNIT} 2>/dev/null)"
         if [ "$_canreload" = "CanReload=no" ]; then
            # The reload action falls back to the sysv init script just in case
            # the systemd service file does not (yet) support reload for a
            # specific service.
            run_via_sysvinit
         else
            exec systemctl $sctl_args reload "${UNIT}"
         fi
         ;;
      force-stop)
         exec systemctl --signal=KILL kill "${UNIT}"
         ;;
      force-reload)
         _canreload="$(systemctl -p CanReload show ${UNIT} 2>/dev/null)"
         if [ "$_canreload" = "CanReload=no" ]; then
            exec systemctl $sctl_args restart "${UNIT}"
         else
            exec systemctl $sctl_args reload "${UNIT}"
         fi
         ;;
      *)
         # We try to run non-standard actions by running
         # the init script directly.
         run_via_sysvinit
         ;;
   esac
fi
```

如果is_systemd的字串长为0则是通过/etc/init.d下对应的脚本来运行
```bash

SERVICEDIR="/etc/init.d"
... ...
run_via_sysvinit() {
   # Otherwise, use the traditional sysvinit
   if [ -x "${SERVICEDIR}/${SERVICE}" ]; then
      exec env -i LANG="$LANG" LANGUAGE="$LANGUAGE" LC_CTYPE="$LC_CTYPE" LC_NUMERIC="$LC_NUMERIC" LC_TIME="$LC_TIME" LC_COLLATE="$LC_COLLATE" LC_MONETARY="$LC_MONETARY" LC_MESSAGES="$LC_MESSAGES" LC_PAPER="$LC_PAPER" LC_NAME="$LC_NAME" LC_ADDRESS="$LC_ADDRESS" LC_TELEPHONE="$LC_TELEPHONE" LC_MEASUREMENT="$LC_MEASUREMENT" LC_IDENTIFICATION="$LC_IDENTIFICATION" LC_ALL="$LC_ALL" PATH="$PATH" TERM="$TERM" "$SERVICEDIR/$SERVICE" ${ACTION} ${OPTIONS}
   else
      echo "${SERVICE}: unrecognized service" >&2
      exit 1
   fi
}
```

init.d目录下脚本示例：

```bash
$ ll
total 364
drwxr-xr-x   2 root root  4096 5月  15 13:49 ./
drwxr-xr-x 148 root root 12288 5月  15 14:02 ../
-rwxr-xr-x   1 root root  2243 2月  10  2016 acpid*
-rwxr-xr-x   1 root root  5336 4月  15  2016 alsa-utils*
-rwxr-xr-x   1 root root  2014 12月 29  2014 anacron*
-rwxr-xr-x   1 root root  8087 6月  11  2018 apache2*
... ...
```

## systemctl

systemd是Linux系统最新的初始化系统(init),作用是提高系统的启动速度，尽可能启动较少的进程，尽可能更多进程并发启动。该命令的启动配置文件是在/lib/systemd/system和/etc/systemd/system目录下。

systemd对应的进程管理命令是systemctl, systemctl命令兼容了service即systemctl也会去/etc/init.d目录下，查看，执行相关程序

```bash
# service命令启动redis脚本
systemctl redis start
# 直接启动redis脚本
/etc/init.d/redis start
# 开机自启动
systemctl enable redisser
```

在/lib/systemd/system和/etc/systemd/system目录下主要有四种类型文件.mount,.service,.target,.wants

.mount文件： 定义了一个挂载点，Mount节点里配置了What,Where,Type三个数据项等同于以下命令：mount -t What Where Type

.service文件: 定义了一个服务，分为[Unit]，[Service]，[Install]三个小节

.target文件：定义了一些基础的组件，供.service文件调用

.wants文件：定义了要执行的文件集合，每次执行，.wants文件夹里面的文件都会执行

**查看系统上上所有的服务**

命令格式：systemctl [command] [--type=TYPE] [--all]

command： 
- list-units：依据unit列出所有启动的unit。
- --all 会列出没启动的unit; 
- list-unit-files，列出启动文件列表
- --type=TYPE  主要有service, socket, target
  
名称 | 说明
:---|:---
systemctl|	列出所有的系统服务
systemctl list-units |	列出所有启动unit
systemctl list-unit-files	| 列出所有启动文件
systemctl list-units --type=service --all|	列出所有service类型的unit
systemctl list-units --type=service --all grep cpu	|列出 cpu电源管理机制的服务
systemctl list-units --type=target --all |	列出所有target


**systemctl特殊的用法**

名称 | 说明
:---|:---
systemctl is-active [service name] | 查看服务是否运行
systemctl is-enable [service name] | 查看服务是否设置为开机启动
systemctl mask [service name] |	注销指定服务
systemctl unmask [service name] | 取消注销指定服务

** service 命令与 systemctl 命令对比**

service | systemctl|说明
:---|:--- |:---
service [服务] start	|systemctl start [服务]	启动服务
service [服务] stop	|systemctl stop [服务]	停止服务
service [服务] restart	|systemctl restart [服务]	重启服务
service [服务] status	|systemctl status [服务]	查看服务状态