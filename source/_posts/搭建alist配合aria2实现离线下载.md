---
title: 搭建alist配合aria2实现离线下载
date: 2022-12-16 21:01:00
---
1.aria2搭建
```
apt install wget curl ca-certificates
wget -N git.io/aria2.sh && chmod +x aria2.sh
./aria2.sh
```
按照提示选择就可以了
2.安装alist
一键安装参考官网
我目前使用的是手机搭建的不能使用systemd
只能使用init.d代替了
下载程序
[下载地址](https://github.com/alist-org/alist/releases)

```
wget 文件链接
tar -zxvf 文件名
chmod +x alist
./alist server
#记录用户密码后，按ctrl➕c退出
```
在etc/init.d目录创建文件alist，填入下边的代码
```
#!/bin/sh
# For RedHat and cousins:
# chkconfig: - 99 01
# alist
# processname: /root/alist

### BEGIN INIT INFO
# Provides:          /root/alist
# Required-Start:
# Required-Stop:
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: alist
# Description:       alist
### END INIT INFO

cmd="/root/alist "server""

name=$(basename $(readlink -f $0))
pid_file="/var/run/$name.pid"
stdout_log="/var/log/$name.log"
stderr_log="/var/log/$name.err"

[ -e /etc/sysconfig/$name ] && . /etc/sysconfig/$name

get_pid() {
    cat "$pid_file"
}

is_running() {
    [ -f "$pid_file" ] && ps $(get_pid) > /dev/null 2>&1
}

case "$1" in
    start)
        if is_running; then
            echo "Already started"
        else
            echo "Starting $name"
            
            $cmd >> "$stdout_log" 2>> "$stderr_log" &
            echo $! > "$pid_file"
            if ! is_running; then
                echo "Unable to start, see $stdout_log and $stderr_log"
                exit 1
            fi
        fi
    ;;
    stop)
        if is_running; then
            echo -n "Stopping $name.."
            kill $(get_pid)
            for i in $(seq 1 10)
            do
                if ! is_running; then
                    break
                fi
                echo -n "."
                sleep 1
            done
            echo
            if is_running; then
                echo "Not stopped; may still be shutting down or shutdown may have failed"
                exit 1
            else
                echo "Stopped"
                if [ -f "$pid_file" ]; then
                    rm "$pid_file"
                fi
            fi
        else
            echo "Not running"
        fi
    ;;
    restart)
        $0 stop
        if is_running; then
            echo "Unable to stop, will not attempt to start"
            exit 1
        fi
        $0 start
    ;;
    status)
        if is_running; then
            echo "Running"
        else
            echo "Stopped"
            exit 1
        fi
    ;;
    *)
    echo "Usage: $0 {start|stop|restart|status}"
    exit 1
    ;;
esac
exit 0
```
如果文件不在root目录，记得修改里边的路径

然后执行
```
cd /etc/init.d
sudo update-rc.d alist defaults
```
可以参考这个文章 [CSDN](http://t.csdn.cn/doxT0)




