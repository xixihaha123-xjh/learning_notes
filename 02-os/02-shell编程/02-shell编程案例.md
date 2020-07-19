脚本工具功能概述：

需求描述：

1. 实现一个脚本工具，该脚本提供类似supervisor功能，可以对进程进行管理
2. 一键查看所有进程运行状态
3. 单个或批量启动进程，单个或批量停止进程
4. 提供进程分组功能，可以按组查看进程运行状态，可以按组启动或停止该组内所有进程



shell脚本操作数据库

（1）安装mysql数据库

```

```









# linux Shell脚本案例

shell 脚本学习 

变量 （包括环境变量）+ 运算符 + 正则表达式 + 流程控制（条件判断式语句）+ 函数 + 常用工具（cut +　find）文本处理三剑客

## 00 课程大纲

1. 服务器系统配置初始化

2. Linux系统发送告警邮件
3. 批量创建100个用户并设置密码
4. 一键查询服务器资源利用率
5. 找出占用CPU/内存过高的进程
6. 查看网卡实时流量
7. 监控100台服务器磁盘利用率
8. 批量检查网站是否异常
9. 批量主机执行命令
10. 一键部署LNMP网络平台
11. 监控Mysql主从同步状态是否异常
12. Mysql数据库备份
13. Nginx访问日志分析
14. Nginx访问日志自动按天切割
15. 自动发布JAVA项目(Tomcat)
16. 自动发布PHP项目
17. DOS攻击防范（自动屏蔽攻击ip）
18. 入侵检测与告警

## 1 服务器系统配置初始化

1. 背景：新购买的服务器已安装Linux操作系统
2. 需求：

```
01 设置时区并同步时间
02 禁用selinux
03 清空防火墙默认策略，关闭防火墙
04 历史命令显示操作时间
05 禁止root远程登录
06 禁止定时任务发送邮件
07 设置最大打开文件数
08 减少swap使用
09 系统内核参数优化
10 安装系统性能分析工具及其他
```

3. 常用命令

```shell
# 1 查看操作系统的版本
cat /etc/redhat-release                 # 查看操作系统的版本
CentOS Linux release 7.6.1810 (Core)

# 2 ntp时间同步 - client开了ntpd服务造成 NTP socket is in use
ntpdate time.windows.com
4 May 11:27:07 ntpdate[2343]: the NTP socket is in use, exiting
# 3 关闭ntpd
service ntpd stop
Redirecting to /bin/systemctl stop ntpd.service
# 4 ntp时间同步，ntp服务器 time.windows.com
ntpdate time.windows.com
4 May 11:27:50 ntpdate[2470]: adjust time server 40.81.94.65 offset -0.008375 sec

# 5 查看系统启动的定时任务
crontab -l
*/1 * * * * /usr/local/qcloud/stargate/admin/start.sh > /dev/null 2>&1 &
0 0 * * * /usr/local/qcloud/YunJing/YDCrontab.sh > /dev/null 2>&1 &

# 6 添加定时任务
crontab -e

# 7 关闭selinux
vim /etc/selinux/config           # 设置: SELINUX=disabled

# 
```



基于数据库备份还是基于表备份



## 5 一键查询服务器资源利用率

### 5.1 web服务器访问较慢

1. 服务器利用率饱和

2. 网络问题

### 5.2 shell脚本查看资源利用率

1. CPU ----- 60%
2. 内存
3. 磁盘IO
4. 网络状况 tcp连接状态 ------ 当前服务器并发状况

### 5.3 linux 命令

```shell
# 查看cpu利用率
top
vmstat
# 查看内存
free -m
# 查看硬盘
df -h
# tcp连接状态
netstat -atnp

# awk格式化输出
# awk以 / 开头
# vmstat输出参数
# $NF
# free -m 输出参数

```

### 5.4 与linux性能优化实战练习

```shell
➜  ~ uptime
 00:05:17 up 119 days,  1:05,  4 users,  load average: 0.10, 0.21, 0.13
 ###### 参数分析 ###### 
 00:05:17               # 当前时间            
 up 119 days,  1:05     # 系统运行时间
 4 users                # 正在登录用户数
 load average: 0.10, 0.21, 0.13  # 过去1分钟、5分钟、15分钟的平均负载（Load Average）
 
shell执行查看输出过程： bash -x script.sh
```



```shell
#!/bin/bash

function cpu()
{
    util=$(vmstat | awk '{if(NR==3) print $13+$14}')
    iowait=$(vmstat | awk '{if(NR==3) print $16}')
    echo "CPU - 使用率: ${util}% 等待磁盘IO响应使用率: ${iowait}%"
}

function memory()
{
    total=$(free -m | awk '{if(NR==2) printf "%.1f", $2/1024}')
    used=$(free -m | awk '{if(NR==2) printf "%.1f", ($2-$NF)/1024}')
    available=$(free -m | awk '{if(NR==2) printf "%.1f", $NF/1024}')
    echo "内存总大小:$total, 已使用:${used}, 剩余:${available}"
}

function disk()
{
    fs=$(df -h | awk '/^\/dev/{print $1}')
    for p in $fs; do
        mounted=$(df -h | awk -v p=$p '$1==p{print $NF}')
        size=$(df -h | awk -v p=$p '$1==p{print $2}')
        used=$(df -h | awk -v p=$p '$1==p{print $3}')
        used_percent=$(df -h | awk -v p=$p '$1==p{print $5}')
        echo "${p} - 挂载点: ${mounted}, 总大小:${size}, 已使用: ${used}, 使用率: ${used_percent}"
    done
}


function tcp_connect()
{
    summary=$(netstat -antp | awk '{a[$6]++}END{for(i in a)printf i":"a[i]" "}')
    echo "tcp连接状态 -${summary}"

}

cpu
memory
disk
tcp_connect
```



## 6 找出占用CPU/内存过高的进程

```shell
➜  ps -eo pid,pcpu,pmem,args --sort=-pcpu | head -n 10

  PID %CPU %MEM COMMAND
20815  0.3 17.2 /usr/sbin/mysqld
 1480  0.2  0.6 barad_agent
23671  0.1  0.9 /usr/local/qcloud/YunJing/YDEyes/YDService
```

```
➜  ps -eo pid,pcpu,pmem,args --sort=-pmem | head -n 10

  PID %CPU %MEM COMMAND
20815  0.3 17.2 /usr/sbin/mysqld
  401  0.0  3.6 /usr/lib/systemd/systemd-journald
16154  0.0  2.4 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```



```shell
#/bin/bash

echo "-----------------------------cpu top 5--------------------------"
ps -eo pid,pcpu,pmem,args --sort=-pcpu | head -n 5

echo "-----------------------------mem top 5--------------------------"
ps -eo pid,pcpu,pmem,args --sort=-pmem | head -n 5
```



```shell
shell 排序默认按字符排序
➜  shell ps aux | awk '{print $4}' | sort  -r | head -n 5
%MEM
3.6
2.4
2.2
17.2
```

## 7 查看网卡实时流量

```shell
ifconfig

iftop

cat /proc/net/dev
cat /proc/net/dev | awk '/eth0/{print $2}'
cat /proc/net/dev | awk '/eth0/{print $10}'
```



```shell
%s 占位符

#!/bin/bash

NIC=$1
echo -e "In ----------Out"
while true; do
    OLD_IN=$(awk '$0~" '$NIC'"{print $2}' /proc/net/dev)
    OLD_OUT=$(awk '$0~" '$NIC'"{print $10}' /proc/net/dev)
    sleep 1
    NEW_IN=$(awk '$0~" '$NIC'"{print $2}'  /proc/net/dev)
    NEW_OUT=$(awk '$0~" '$NIC'"{print $10}' /proc/net/dev)
    IN=$(printf "%.1f%s" "$((($NEW_IN-$OLD_IN)/1024))" "kb/s")
    OUT=$(printf "%.1f%s" "$((($NEW_OUT-$OLD_OUT)/1024))" "kb/s")
    echo "$IN    $OUT"
    sleep 1
done
```



## 8 监控100台服务器磁盘利用率

