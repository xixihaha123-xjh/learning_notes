# 1 背景

（1） 命令语言与程序设计语言

1. 作为命令语言，交互式解释和执行用户输入的命令或者自动地解释和执行预先设定好的一连串的命令 

2. shell 作为程序设计语言，定义了各种变量和参数，并提供了许多在[高级语言](https://baike.baidu.com/item/高级语言)中才具有的控制结构，包括循环和分支 

3. 区分什么是命令语言什么是程序设计语言。

（2）强类型与弱类型语言

1. 弱类型语言也称为弱类型定义语言。与强类型定义相反。像vb，php等就属于弱类型语言·

（3）在[排序算法](https://baike.baidu.com/item/排序算法)中，Shell是[希尔排序](https://baike.baidu.com/item/希尔排序)的名称。

（4）在一般情况下，人们并不区分 Bourne Shell 和 Bourne Again Shell，所以，像 **#!/bin/sh**，它同样也可以改为 **#!/bin/bash**。**#!** 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。

# 2 基础

（1）课程大纲

1. Shell基础
2. Bash变量
3. Shell运算符
4. 环境变量配置文件
5. 正则表达式
6. 流程控制语句

## 2.1 Bash变量

### 2.1.1 变量及其命名规则

（1）什么是变量

1. 变量是计算机内存的单元，其中存放的值可以改变

2. 变量让你能够把程序中准备使用的每一段数据都赋给一个简短、易于记忆的名字，因此它们十分有用

（2）变量命名规则

1. 变量名必须以字母或下划线打头，名字中间只能有字母、数字和下划线组成
2. 变量名的长度不得超过255个字符
3. 变量名要有含义
4. 变量名在有效的范围内必须是唯一的
5. 变量按照存储类型数据分类，可分为：字符串类型、整型、浮点型、日期型 .....；**但在Bash中，变量的默认类型都是字符串型**

### 2.1.2 变量分类

1. 用户自定义变量
2. 环境变量：这种变量主要保存的是和系统操作环境相关的数据。变量可以自定义，但是对系统生效的环境变量名和变量作用是固定的，也就是说想让系统环境生效，系统默认定义好的一些环境变量，只能改变它的值，它的名字不能随便改
3. 位置参数变量：这种变量主要是用来向脚本中传递参数或数据的，变量名不能自定义，变量作用是固定的
4. 预定义变量：是Bash中已经定义好的变量，变量名不能自定义，变量作用也是固定的

### 2.1.3 用户自定义变量

（1）定义变量

> 变量名=变量值
>
> 例如：x=Nihaowa

1. Bash等号两侧不能加空格，否则系统认为是一条命令

```shell
[root@Nihaowa ~]# name=Nihaowa
[root@Nihaowa ~]# name = "Nihaowad"
-bash: name: command not found
```

2. 变量不能以数字开头
3. 变量值里边如果有空格，变量需要用双引号括起来
4. 定义变量时，变量名不加美元符号（$，PHP语言中变量需要），如：

```shell
your_name="runoob.com"
```

（2）变量调用

> echo $变量名

1. shell变量没有数据类型的区分

2. Shell 把任何存储在变量中的值，皆视为以字符组成的“字符串”。 

```shell
[root@Nihaowa ~]# echo $name
Nihaowa
```

```shell
[root@Nihaowa ~]# y=1
[root@Nihaowa ~]# x=2
[root@Nihaowa ~]# z=$x+$y
[root@Nihaowa ~]# echo $z
2+1
```

（3）变量叠加

```shell
[root@Nihaowa ~]# x=123
[root@Nihaowa ~]# x="$x"456
[root@Nihaowa ~]# echo $x
123456

[root@Nihaowa ~]# x=${x}789
[root@Nihaowa ~]# echo $x
123456789
```

（4）变量查看

> set：作用主要是显示系统中已经存在的shell变量,以及设置shell变量的新变量值 
>
> -u：如果设定此选项，调用未声明变量时会报错（默认无任何提示）

```shell
[root@Nihaowa ~]# echo $a

[root@Nihaowa ~]# set -u
[root@Nihaowa ~]# echo $a
-bash: a: unbound variable
```

（5）变量删除

```shell
unset 变量名                                    # 变量名前不加 $
```

### 2.1.4 环境变量

（1）环境变量与用户自定义变量的区别？

1. 环境变量是全局变量，环境变量在当前Shell和这个Shell的所有子Shell中生效
2. 用户自定义变量是局部变量，用户自定义变量只在当前的Shell中生效
3. 环境变量可以自定义，但是对系统生效的环境变量名和变量作用是固定的

（2）设置环境变量的两种方法：

1. 方法一

```shell
export 变量名=变量值
```

2. 方法二

```shell
变量名=变量值
export 变量名
```

（3）查看环境变量 pstree：查看进程树

> set：查看所有变量
>
> env：查看环境变量

```shell
[root@Nihaowa ~]# export name=xixihaha123
[root@Nihaowa ~]# env
name=xixihaha123
```

（4）删除变量

> unset 变量名

```shell
[root@Nihaowa ~]# unset name
```

（5）常用环境变量

1. linux环境变量

```shell
XDG_SESSION_ID=59
HOSTNAME=Nihaowa                   # 主机名
TERM=xterm                         # 终端环境
SHELL=/bin/bash                    # 当前的shell
HISTSIZE=3000                      # 历史命令条数
SSH_CLIENT=113.139.88.209 2187 22  # ssh远程登录客户端ip
SSH_TTY=/dev/pts/0                 # ssh连接的终端是pts/0
JRE_HOME=/usr/jdk1.8.0_161/jre   
USER=root                          # 当前登录的用户
CLASS_PATH=
MAIL=/var/spool/mail/root          # 邮箱地址
# PATH路径-系统搜索命令的路径
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/www/wdlinux/mysql/bin:/usr/local/lib:/usr/jdk1.8.0_161/bin:/usr/jdk1.8.0_161/jre/bin:/root/bin
PWD=/root
JAVA_HOME=/usr/jdk1.8.0_161         # 当前所在路径
LANG=en_US.utf8                     # 语言环境
SHLVL=1
HOME=/root
LOGNAME=root
SSH_CONNECTION=113.139.88.209 2187 172.17.0.15 22
LESSOPEN=||/usr/bin/lesspipe.sh %s
PROMPT_COMMAND=history -a; printf "\033]0;%s@%s:%s\007" "${USER}" "${HOSTNAME%%.*}" "${PWD/#$HOME/~}"
XDG_RUNTIME_DIR=/run/user/0
HISTTIMEFORMAT=%F %T 
_=/usr/bin/env
```

（6）PATH环境变量

1. 系统搜索命令的路径

2. 查看PATH环境变量

```shell
echo $PATH
/apps/sylar/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
```

3. 增加PATH变量的值

```shell
PATH="$PATH":/root/sh
```

（7）PS1环境变量

```shell
[root@Nihaowa ~]# echo $PS1
[\u@\h \W]\$

\d	代表日期，格式为weekday month date，例如："Mon Aug 1"
\H	完整的主机名称。例如：我的机器名称为：fc4.linux，则这个名称就是fc4.linux
\h	仅取主机的第一个名字，如上例，则为fc4，.linux则被省略
\t	显示时间为24小时格式，如：HH：MM：SS
\T	显示时间为12小时格式
\A	显示时间为24小时格式：HH：MM
\u	当前用户的账号名称
\v	BASH的版本信息
\w	完整的工作目录名称。家目录会以 ~代替
\W	利用basename取得工作目录名称，所以只会列出最后一个目录
\#	下达的第几个命令
\$	提示字符，如果是root时，提示符为：# ，普通用户则为：$
\[	字符"["
\]	字符"]"
\!	命令行动态统计历史命令次数
```

（8） PS2环境变量

1. PS2 是副提示符变量，默认值是''> ''，PS2 一般使用于命令行里较长命令的换行提示信息

（9） 语系变量

```shell
[root@Nihaowa ~]# locale                 # 查询当前系统语系
LANG=en_US.utf8                          # 定义系统主语系的变量
LC_ALL=                                  # 定义整体语系的变量

locale -a | more                         # 查看Linux支持的所有语系
cat /etc/sysconfig/i18n                  # 查询系统默认语系
```

1. Linux中文支持

* 前提条件，正确安装的中文字体和中文语系
* 如果有图形界面，可以正确支持中文显示
* 如果使用第三方远程工具，只要语系设定正确，可以支持中文显示
* 如果使用纯字符界面，必须使用第三方插件（如zhcon等）

### 2.1.5 位置参数变量

| 位置参数变量 | 作用                                                         |
| :----------: | ------------------------------------------------------------ |
|      $n      | n为数字，0代表命令本身，1-9代表第一到第九个参数，十以上的参数需要用大括号包含，如${10}，主要用于用户向脚本中传递值，与C语言中main函数的参数类似 |
|      $*      | 这个变量代表命令行中所有的参数，$* 把所有的参数看成一个整体  |
|      $@      | 这个变量也代表命令行中所有的参数，不过 $@ 把每个参数区分对待 |
|      $#      | 这个变量代表命令行中所有参数的个数                           |

（1） $n

```shell
#!/bin/bash
num1=$1
num2=$2
sum=$(( $num1 + $num2 ))                # 变量sum的和是num1叫num2
echo $sum                               # 打印变量sum的值
# 输出------------------------------------------------------------
➜  shell ./example.sh 1 3
4
```

（2） `$*`与 `$@`

```shell
#!/bin/bash
echo "a total of $# parameters"         # 使用 $# 代表所有参数的个数
echo "the parameters is: $*"            # 使用 $* 代表所有的参数
echo "the parameters is: $@"            # 使用 $@ 也代表所有的参数
# 输出--------------------------------------------------------------
➜  shell ./example.sh 1 2 3 5
a total of 4 parameters
the parameters is: 1 2 3 5
the parameters is: 1 2 3 5
```

```shell
#!/bin/bash
for x in "$*"                           # 这个变量代表命令行中所有的参数
	do
		echo "the parameters is: $x"  
	done

for y in "$@"                           # 这个变量也代表命令行中所有的参数
	do
		echo "parameter:$y"
	done
	
# 输出-------------------------------------------------
➜  shell ./example.sh 1 2 3 5
the parameters is: 1 2 3 5
parameter:1
parameter:2
parameter:3
parameter:5
```

### 2.1.6 预定义变量

（1） 预定义变量

| 变量 | 作用                                                         |
| :--: | ------------------------------------------------------------ |
|  $?  | 最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；如果这个变量的值为非0（具体是哪个数，有命令自己来决定），则证明上一个命令执行不正确 |
|  $$  | 当前进程的进程号(PID)                                        |
|  $!  | 后台运行的最后一个进程的进程号(PID)                          |

```shell
#!/bin/bash
echo "process id: $$"
find /root -name hello.sh &             # Ctrl+z 放入后台后是暂停的, & 放入后台,是一直执行的
echo "background process id: $!"
# 输出----------------------------------------------------------------
➜  shell ./example.sh
process id: 29106
background process id: 29107
```

（2）read命令 - 接收键盘输入

```shell
read [选项][变量名]
- 选项
-p: 提示信息,在等待read输入时,输出提示信息
-t: 秒数,read命令会一直等待用户输入,使用此选项可以指定等待时间
-n: 字符数,只接受指定的字符数,就会执行
-s: 隐藏输入的数据,适用于机密信息的输入
```

```shell
#!/bin/bash
read -p "please input your name: " -t 30 name
echo $name

read -p "please input your passwd: " -s passwd
echo -e "\n"
echo $passwd

read -p "please input your sex [M/F]: " -n 1 sex
echo -e "\n"
echo $sex
# 输出--------------------------------------------------------------------
➜  shell ./example.sh
please input your name: xixihaha123
xixihaha123
please input your passwd:

123
please input your sex [M/F]: M

M
```

```shell
echo -e "\c"(-e 开启转意，\c表示不换行)
[root@Nihaowa shell]# echo ""                    # echo ""本身会输出换行
[root@Nihaowa shell]# echo -e "\n"               # 换行+换行 
```

### 2.1.7 变量替换和测试

（1）变量替换

| 语法                       | 说明                                                         |
| -------------------------- | ------------------------------------------------------------ |
| ${变量#匹配规则}           | 从变量**开头**进行规则匹配，将符合**最短**的数据删除         |
| ${变量##匹配规则}          | 从变量**开头**进行规则匹配，将符合**最长**的数据删除         |
| ${变量%匹配规则}           | 从变量**尾部**进行规则匹配，将符合**最短**的数据删除         |
| ${变量%%匹配规则}          | 从变量**尾部**进行规则匹配，将符合**最长**的数据删除         |
| ${变量/旧字符串/新字符串}  | 变量内容符合旧字符串规则，则**第一个**旧字符串会被新字符串取代 |
| ${变量//旧字符串/新字符串} | 变量内容符合旧字符串规则，则**全部的**旧字符串会被新字符串取代 |

```shell
➜  ~ variable_1="I love you,Do you love me"
➜  ~ echo $variable_1
I love you,Do you love me

➜  ~ var1=${variable_1#*ov}
➜  ~ echo $var1
e you,Do you love me

➜  ~ var2=${variable_1##*ov}
➜  ~ echo $var2
e me

➜  ~ var3=${variable_1%ov*}
➜  ~ echo $var3
I love you,Do you l

➜  ~ var4=${variable_1%%ov*}
➜  ~ echo $var4
I l

➜  ~ echo $PATH
/apps/sylar/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
➜  ~ var5=${PATH/bin/BIN}
➜  ~ echo $var5
/apps/sylar/BIN:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin

➜  ~ var6=${PATH//bin/BIN}
➜  ~ echo $var6
/apps/sylar/BIN:/usr/local/sBIN:/usr/local/BIN:/usr/sBIN:/usr/BIN:/root/BIN
```

（2）变量测试

![变量测试](pic\51-变量测试.png)

## 2.2 字符串

### 2.2.1 计算字符串长度

1. 语法

```shell
${#string}
expr length "$string        # string 有空格，则必须加双引号
```

2. 案例

```shell
[root@Nihaowa shell]# var="Hello World"
[root@Nihaowa shell]# echo len=${#var}
len=11
[root@Nihaowa shell]# echo len=`expr length "$var"`
len=11
```



### 2.2.2 获取子串在字符串中的索引位置

1. 获取子串在字符串中索引位置，将子串拆分为一个个字符去匹配字符串中的字符，返回第一个匹配的字符索引
2. 语法 

```shell
expr index $string $substring
```

3. 案例

```shell
[root@Nihaowa shell]# var="quickstart is a app"
[root@Nihaowa shell]# echo index=`expr index "$var" start`
index=6
[root@Nihaowa shell]# echo index=`expr index "$var" unique`
index=1
[root@Nihaowa shell]# echo index=`expr index "$var" cnk`
index=4

[root@Nihaowa shell]# var="abc"
[root@Nihaowa shell]# echo index=`expr index "$var" d`
index=0
[root@Nihaowa shell]# echo index=`expr index "$var" ac`
index=1
[root@Nihaowa shell]# echo index=`expr index "$var" ca`
index=1
```

### 2.2.3 计算子串长度

1. 计算子串的长度，必须从字符串头开始匹配

2. 语法

```shell
expr match $string substr
```

3. 案例

```shell
➜  ~ var="quickstart is a app"
➜  ~ echo sub_len=`expr match "$var" app`
sub_len=0
➜  ~ echo sub_len=`expr match "$var" quick`
sub_len=5
➜  ~ echo sub_len=`expr match "$var" uick`
sub_len=0
```

### 2.2.4 抽取子串

1. 语法

|        | 语法                                 | 说明                             |
| ------ | ------------------------------------ | -------------------------------- |
| 方法一 | ${string:position}                   | 从string中position开始           |
| 方法二 | ${string:position:length}            | 从position开始，匹配长度为length |
| 方法三 | ${string: -position}                 | 从右边开始匹配                   |
| 方法四 | ${string:(position)}                 | 从左边开始匹配                   |
| 方法五 | expr substr string  ​position  ​length | 从position开始，匹配长度为length |

```shell
[root@Nihaowa shell]# var="kafka hadoop yarm mapreduce"
[root@Nihaowa shell]# echo substr=${var:6}
substr=hadoop yarm mapreduce                                 # 索引位置从 0 开始计数

[root@Nihaowa shell]# echo substr=${var:6:6}
substr=hadoop

[root@Nihaowa shell]# echo substr=${var: -9}                  # 索引位置从 -1 开始计数
substr=mapreduce
[root@Nihaowa shell]# echo substr=${var:(-9)}
substr=mapreduce
[root@Nihaowa shell]# echo substr=${var: -9:4}
substr=mapr

[root@Nihaowa shell]# echo substr=`expr substr "$var" 7 8`    # 索引位置从 1 开始计数
substr=hadoop y
```

### 2.2.5 字符串处理脚本

（1）需求描述

```
变量 string="Bigdata process framwork is Hadoop, Hadoop is an open source project"
执行脚本后，打印输出string字符串变量，并给出用户以下选项
echo "(1) 打印string长度"
echo "(2) 删除字符串中所有的Hadoop"
echo "(3) 替换第一个Hadoop为Mapreduce"
echo "(4) 替换全部Hadoop为Mapredeuce"
用户输入数字1|2|3|4，可执行对应项的功能：输入q|Q则退出交互模式
```

（2）思路分析

1. 划分功能模块，并编写函数

* function print_tips
* function len_of_string
* function del_hadoop
* function rep_hadoop_mapreduce_first
* function rep_hadoop_mapredece_all

2. 实现第一步所定义的功能函数
3. 程序主流程设计

（3）实现

1. 实现第一步所定义的功能函数

```shell
string="Bigdata process framwork is Hadoop, Hadoop is an open source project"

function print_tips
{
    echo "*************************************"
    echo "(1) 打印string长度"
    echo "(2) 删除字符串中所有的Hadoop"
    echo "(3) 替换第一个Hadoop为Mapreduce"
    echo "(4) 替换全部Hadoop为Mapredeuce"
    echo "*************************************"
}

function len_of_string
{
    echo "${#string}"
}

function del_hadoop
{
   echo "${string//Hadoop/}"
}

function rep_hadoop+mapreduce_first
{
   echo "${string/Hadoop/Mapreduce}"
}

function rep_hadoop+mapreduce_all
{
   echo "${string//Hadoop/Mapreduce}"
}
```

2. 程序主流程设计

```shell
while true
do
    echo "[string=$string]"
    echo
    print_tips
    read -p "Pls input your choice(1|2|3|4|q|Q): "  choice

    case $choice in
        1)
            len_of_string
            ;;
        2)
            del_hadoop
            ;;
        3)
            rep_hadoop+mapreduce_first
            ;;
        4)
            rep_hadoop+mapreduce_all
            ;;
        q|Q)
            exit
            ;;
        *)
            echo "Error,input only in{1|2|3|4|q|Q}"
            ;;
    esac
done
```



## 2.3 运算符

1. shell变量是弱类型，默认字符串类型

```shell
[root@Nihaowa shell]# num_1=10
[root@Nihaowa shell]# echo num_2=$num_1+20
num_2=10+20                                           # shell默认变量类型为字符串类型
```

### 2.3.1 declare命令

（1） declare 命令

```shell
declare [+/-] [选项] 变量名
选项
 -： 给变量设定类型属性
 +： 取消变量的类型属性
-a: 将变量声明为数组型
-i: 将变量声明为整数型(integer)
-x: 将变量声明为环境变量,声明一个变量为环境变量，表示这个变量与该终端无关
-r: 将变量声明为只读变量
-p: 显示指定变量的被声明的类型
-f: 显示此脚本前定义过的所有函数及内容
-F: 仅显示此脚本前定义过的函数名
```

（2）把变量声明为数值型

```shell
# 把变量声明为数值型
➜  shell a=1
➜  shell b=2
➜  shell declare -i c=$a+$b      # 声明变量c的类型是整数型,它的值是 a 和 b 的和
➜  shell echo $c
3

➜  shell declare -p c            # 显示指定变量的被声明的类型
typeset -i c=3
```

（3）声明数组变量

```shell
# 定义数组
[root@Nihaowa shell]# movie[0]=tp
[root@Nihaowa shell]# movie[1]=zp
[root@Nihaowa shell]# declare -a movie[2]=live
# 查看数组
[root@Nihaowa shell]# echo ${movie}
tp
[root@Nihaowa shell]# echo ${movie[*]}
tp zp live
[root@Nihaowa shell]# echo ${movie[1]}             # index从0开始
zp

[root@Nihaowa shell]# declare -a array
[root@Nihaowa shell]# array=("jones" "mike" "kobe" "jorden")
[root@Nihaowa shell]# echo ${array[@]}             # 输出所有数组的元素
jones mike kobe jorden
[root@Nihaowa shell]# echo ${array[1]}             # 输出第一个数组元素
mike
[root@Nihaowa shell]# echo ${#array[@]}            # 数组元素的个数
4
[root@Nihaowa shell]# echo ${#array[0]}            # 第一个元素的长度
5
```

（4）声明环境变量

```shell
declare -x test=123                           # 和export作用相似,但其实是declare命令的作用
```

（5）声明变量只读属性

```shell
[root@Nihaowa shell]# declare -r Nihaowa
# 给test赋予只读属性,但是请注意只读属性会让变量不能修改不能删除,甚至不能取消只读属性

[root@Nihaowa shell]# declare -p Nihaowa
declare -rx Nihaowa="123"
[root@Nihaowa shell]# Nihaowa=234
bash: Nihaowa: readonly variable                        # 不能修改
[root@Nihaowa shell]# unset Nihaowa
bash: unset: Nihaowa: cannot unset: readonly variable   # 不能删除
```

### 2.3.2 数值运算方法

（1）方法一：

```
declare -i c=$a+$b
```

（2）方法二：expr或let数值运算工具

```shell
[root@Nihaowa shell]# a=1
[root@Nihaowa shell]# b=2
[root@Nihaowa shell]# echo c=$(($a + $b))
c=3
[root@Nihaowa shell]# echo c=$(expr $a + $b)              # "+"号左右两侧必须有空格
c=3                                                       # c的值是a和b的和
```

（3）方法三：

````shell
[root@Nihaowa shell]# echo c=$[$a+$b]                     # 注意等号两边不能有空格
c=3
[root@Nihaowa shell]# echo c=$(( $a+$b ))
c=3
# 注意与 $ 单括号区分 ---------------------------------------------------------
[root@Nihaowa shell]# echo d=$(date)
d=Sun Jun 21 19:52:06 CST 2020
````

（4）运算符优先级

![运算符优先级](pic\50-运算符优先级.png)

（5）案例

```shell
[root@Nihaowa shell]# num_1=30
[root@Nihaowa shell]# num_2=50
[root@Nihaowa shell]# expr $num_1 \> $num_2     # |  &  <  <=  >  >= 必须要加转义
0
[root@Nihaowa shell]# expr $num_1 \< $num_2
1
[root@Nihaowa shell]# num_3=`expr $num_1 + $num_2`
[root@Nihaowa shell]# echo $num_3
80
[root@Nihaowa shell]# expr $num_1 * $num_2
expr: syntax error                                        # * 要加转移
[root@Nihaowa shell]# expr $num_1 \* $num_2
1500
[root@Nihaowa shell]# expr $num_1 / $num_2                # 除法取整，为0
0
[root@Nihaowa shell]# expr $num_1 % $num_2
30
```

```shell
# 提示用户输入一个正整数num，然后计算1 + 2 +３＋ .... + num的值：必须对num是否为正整数做判断，不符合应当允许再次输入

#!/bin/bash
while true
do
    read -p "please input a positive number: " num
    expr $num + 1 &> /dev/null

    if [ $? -eq 0 ];then
        if [ `expr $num \> 0` -eq 1 ];then
            for((i=1; i<=$num;i++))
            do
                sum=`expr $sum + $i`
            done
            echo "1+2+3...+$num = $sum"
            exit

        fi
    fi
    echo "error, input enlegal"
    continue
done
```



### 2.3.3 变量测试

（1） 变量测试 -- 变量测试在脚本优化时使用

![变量测试](pic\51-变量测试.png)

（2）测试

```shell
[root@Nihaowa shell]# unset y
[root@Nihaowa shell]# echo x=${y-2}                   # 变量y没有设置,x = 2(新值)
2
[root@Nihaowa shell]# y=""
[root@Nihaowa shell]# echo x=${y-2}                   # 变量y为空, x的值就为空
x=

[root@Nihaowa shell]# y=1
[root@Nihaowa shell]# echo x=${y-2}                   # 变量y设置, x的值就为y设置的值
x=1
```

### 2.3.4 命令替换

（1）作用：一个命令执行结果，作为另一个命令参数的一部分

（2）语法

```shell
方法一:  `command`
方法二:  $(command)
```

（3）例子

1. 获取系统所有用户并输出

```shell
#!/bin/bash
index=1
for user in `cat /etc/passwd | cut -d ":" -f 1`
do
    echo "This is $index user: $user"
    index=$(($index+1))
done
```

2. 根据系统时间计算今年或明年

```shell
[root@Nihaowa]# echo "This is $(date +%Y) year"
This is 2020 year

[root@Nihaowa]# echo "This is $(($(date +%Y) + 1)) year"
This is 2021 year
```

3. 根据系统时间获取今年还剩下多少星期，已经过了多少星期

```shell
[root@Nihaowa]# echo "This year have passed $(date +%j) days"
This year have passed 122 days

[root@Nihaowa]# echo "This year have passed $(($(date +%j)/7)) weeks"
This year have passed 17 weeks

[root@Nihaowa]# echo "There is $((365 - $(date +%j))) days before new year"
There is 243 days before new year

[root@Nihaowa]# echo "There is $(($((365 - $(date +%j)))/7)) weeks before new year"
There is 34 weeks before new year
```

4. 判定nginx进程是否存在，若不存在则自动拉起该进程

```shell
#!/bin/bash

nginx_process_num=$(ps -ef | grep nginx | grep -v grep | wc -l)
if [$nginx_process_num -eq 0];then
	systemctl start ngix
fi
```

（4）总结

1. `$(()) ` 主要用来进行整数运算，包括加减乘除，引用变量前面可以加 `$` ,也可以不加`$`

```shell
$(((100 + 30) / 13 ))

[root@Nihaowa]# num_1=10
[root@Nihaowa]# bum_2=20
[root@Nihaowa]# echo "$(($num_1 + $num_2))"
30
```

### 2.3.5 bash内建运算器

1. bc是bash内建的运算器，支持浮点数运算
2. 内建变量scale可以设置，默认为0

```shell
[root@Nihaowa]# bc
scale=5                                   # 设置bc浮点数精度
23 / 5
4.60000

[root@Nihaowa]# echo "scale=4; 28.5/3.5" | bc
8.1428
```



## 2.4 环境变量配置文件

### 2.4.1 简介

（1） source命令

1. 修改配置文件后，必须注销重新登录才能生效，使用source命令可以不用重新登录

（2）环境变量配置文件简介

1. PATH、HISTSIZE、PS1、HOSTNAME等环境变量写入对应的环境变量配置文件
2. 环境变量配置文件中主要是定义对系统操作环境生效的系统默认环境变量，如PATH等。

3. 环境变量配置文件都有哪些？

```shell
# 位于/etc目录下的配置文件对所有的用户登录都起作用，位于用户家目录下的配置文件只对对应用户登录才起作用
/etc/profile             # 修改历史命令条数    HISTSIZE 
/etc/profile.d/*.sh
/etc/bashrc              # 修改登录提示符  PS1 
~/.bash_profile          # 每个用户家目录,都有这个文件
~/.bashrc                # 修改别名 alias
```

### 2.4.2 环境变量配置文件的功能

（1） 环境变量配置文件加载顺序

1. 用户正常登陆加载顺序： 

![环境变量调用过程](pic\53-环境变量调用过程.png) 

2. 用户非正常登陆：用户非正常登录时（如用su命令切换用户时，无需输入用户名和密码），配置文件加载顺序如下：/etc/bashrc -> /etc/profile.d/*.sh -> /etc/profile.d/lang.sh -> /etc/sysconfig/i18n -> 命令行界面 

![环境变量调用过程](pic\54-环境变量调用过程.png)

（2） /etc/profile 的作用

```
USER变量:
LOGNAME变量:
MAIL变量:
PATH变量:
HOSTNAME变量:
HISTSIZE变量:
umask
调用/etc/profile.d/*.sh文件
```

（3）umask权限

1. umask 查看系统默认权限
2. 注意：文件最高权限为666，目录最高权限为777，权限不能使用数字进行换算，而必须使用字母；umask定义的权限，是系统默认权限中准备丢弃的权限

 ```
若umask 是022
r 4   w 2   x 1
那么新建的文件权限：
666   rw-rw-rw-
022   ----w--w-
644   rw-r--r--

新建目录权限：
777   rwxrwxrwx
022   ----w--w-
755   rwxr-xr-x
如果umask为022，那么默认文件的权限为644 ，默认目录的权限为755。
 ```

（4）~/.bash_profile的作用

1. 调用了 ~/.bashrc文件
2. 在PATH变量后面加入了 ":$HOME/bin"  这个目录

（5）~/.bashrc 的作用

1. 定义默认别名
2. 调用/etc/bashrc

（6） /etc/bashrc的作用

1. PS1变量
2. umask
3. PATH变量
4. 调用 /etc/profile.d/*.sh 文件

### 2.4.3 其他环境变量配置文件

（1）注销时生效的环境变量配置文件

1.   ~/.bash_logout：退出时需要清除历史命令，将清除历史命令放到该文件中。历史命令不建议清除，如果在命令中输入明文的用户名和密码，建议清除历史命令

（2）~/.bash_histroy：保存历史命令的文件

1.  命令先保存在内存，正确退出，才会写入文件
2.  用history 命令看，与 vi .bash_history 查看文件看的区别：history 记录的命令多后者的多，原因是history 是记录在内存中，包含了本次登录后操作的命令；而后者还未将本次登录操作的命令保存在内 

（3）shell登录信息

1. 本地终端欢迎信息：/etc/issue

![转义符](pic\55-本地终端欢迎信息.png)

（4）远终端欢迎（警告）信息： /etc/issue.net 

```shell
- 转义符 /etc/issue.net 文件中不能使用
- 是否显示此欢迎语，有ssh配置文件/etc/ssh/sshd_config 决定,加入 "Banner /etc/issue.net" 行才能显示,(记得重启SSH服务) service sshd restart
```

（5）登陆后欢迎信息 /etc/motd

1.  不管是本地登录，还是远程登录，都可以显示此欢迎信息 
2.  tty1-6是文本型控制台,7是x-window(图形)控制台 

## 2.5 正则表达式

### 2.5.1 是什么？

（1）正则表达式是用于描述字符排列和匹配模式的一种语法规则。它主要用于字符串的模式分割、**匹配**、查找及替换操作。主要用于模糊匹配。 

（2）通配符：通配符用来匹配符合条件的文件名,通配符是完全匹配

```
*                      匹配任意字符
?                      匹配任意一个内容
[]                     匹配中括号中的一个字符
[!list] [^list]        匹配 除list 中的任意单一字符
[c1-c2]                匹配 c1-c2 中的任意单一字符 如：[0-9] [a-z]
[!c1-c2] [^c1-c2]      匹配不在c1-c2的任意字符
{string1,string2,...}  匹配 sring1 或 string2 (或更多)其一字符串
```

```shell
# 案例
a*b       a与b之间可以有任意长度的任意字符, 也可以一个也没有, 如aabcb, axyzb, a012b, ab
a?b       a与b之间必须也只能有一个字符,可以是任意字符, 如aab, abb, acb, a0b
a[xyz]b   a与b之间必须也只能有一个字符,但只能是 x 或 y 或 z, 如: axb, ayb, azb
a[!0-9]b  a与b之间必须也只能有一个字符,但不能是阿拉伯数字, 如axb, aab, a-b
a[0-9]b   0与9之间必须也只能有一个字符 如a0b, a1b... a9b
a[!0-9]b  如acb adb
a{abc,xyz,123}b  列出aabcb,axyzb,a123b
```



（3）正则表达式与通配符

1. 正则表达式用来在文件中匹配符合条件的字符串，**正则是包含匹配**。grep、awk、sed等命令行可以支持正则表达式；

2. 通配符用来匹配符合条件的文件名，**通配符是完全匹配**。ls、find、cp这些命令不支持正则表达式，所以只能使用shell自己的通配符来进行匹配；

### 2.5.2 正则表达式

（1）正则表达式

![正则表达式](pic\56-正则表达式.png)

（2）`* `   与  `.`

```shell
# Linux系统环境变量 .bashrc 文件添加别名
alias rm='rm -i'
alias vi='vim'
alias grep='grep --color=auto'

"a*"            # 匹配所有内容,包括空白行,前一个字符0此出现或者任意多次
"aa*"           # 匹配至少包含有一个a的行
"aaa*"          # 匹配最少包含两个连续a的字符串

[root@Nihaowa shell]# grep "a*" test.txt        # 列出文件全部行,这个正则没有任何含义
####输出-------------------------------------------------------------
a
aa
aaa

b
bb
bbbb
[root@Nihaowa shell]# grep "aa*" test.txt       # 匹配至少包含有一个a的行
####输出-------------------------------------------------------------
a
aa
aaa

"s..d"  # 会匹配在s和d这两个字母之间一定有两个字符的单词
"s.*d"  # 匹配在s和d字母之间的任意字符
".*"    # 匹配所有内容
[root@Nihaowa shell]# grep "s..d" test.txt 
said
soid
suud

[root@Nihaowa shell]# grep "s.*d" test.txt
said
soid
suud
soooooood
```

（3） `^`     `$`      `[]`     `[^]`

```shell
"^M"         # 匹配以大写 "M" 开头的行
"n$"         # 匹配以小写 "n" 结尾的行
"^$"         # 匹配空白行

"s[ao]id"    # 匹配s和i字母中,要不是a,要不是o
"[0-9]"      # 匹配任意一个数字

"^[a-z]"     # 匹配用小写字母开头的行
"^[^a-z]"    # 匹配不以小写字母开头的行
"^[^a-zA-Z]" # 匹配不以字母开头的行
```

（4） `\`    `\{n\}`   `\{n,\}`     `\{n,m\}`

```shell
".$"         # 任意字符结尾的行
"\.$"        # 匹配使用"." 结尾的行

"a\{3\}"     # 匹配a字母连续出现三次的字符串
"[0-9]\{3\}" # 匹配包含连续的三个数字的字符串

"^[0-9]\{3,\}[a-z]" # 匹配最少用连续三个数字开头的行

"sa\{1,3\}i" # 匹配在字母s和字母i之间有最少一个a,最多三个a
```

（5） 例子

```shell
# 匹配日期格式 YYYY-MM-DD
[0-9]\{4\}-[0-9]\{2\}-[0-9]\{2\}
# 匹配IP地址  \. (\ 表示转义符)
[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}
```



## 2.6 流程控制

### 2.6.1 条件判断式语句

* 作用：在程序中判断某些条件是否成立

（1）按文件类型判断

```shell
# 按照文件类型进行判断
测试选项	      作用
-b 文件名	 判断该文件是否存在，并且是否为块设备文件（是块设备文件为真）
-c 文件名   判断该文件是否存在，并且是否为字符设备文件（是字符设备文件为真）
-d 文件名   判断该文件是否存在，并且是否为目录文件（是目录为真）             # 重要
-e 文件名   判断该文件是否存在（存在为真）                               # 重要
-f 文件名   判断该文件是否存在，并且是否为普通文件（是普通文件为真）          # 重要
-L 文件名	 判断该文件是否存在，并且是否为符号链接文件（是符号链接文件为真）
-p 文件名   判断该文件是否存在，并且是否为管道文件（是管道文件为真）
-s 文件名   判断该文件是否存在，并且是否为非空（非空为真）
-S 文件名	 判断该文件是否存在，并且是否为套接字文件（是套接字文件为真）

# 两种判断格式
test -e /root/install.log
[ -e /root/install.log ]	  # []两边都会有空格

# 举例：
[ -e /root/install.log ]
echo $?

[ -e /root/install.log ] && echo "yes" || echo "no" # 如果该文件存在 就输出yes 否则输出no
```

（2） 按文件权限判断

```shell
测试选项	       作用	
-r 文件	 断该文件是否存在，并且是否该文件拥有读权限（有读权限为真）
-w 文件    判断该文件是否存在，并且是否该文件拥有写权限,不管文件所属者,所属组,只要文件有写权限就返回真
-x 文件	 断该文件是否存在，并且是否该文件拥有执行权限（有执行权限为真）

-u 文件    判断该文件是否存在，并且是否该文件拥有SUID权限（有SUID权限为真）
-g 文件    判断该文件是否存在，并且是否该文件拥有SGID权限（有SGID权限为真）
-k 文件	 判断该文件是否存在，并且是否该文件拥有SBit权限（有SBit权限为真）

[ -w /root/install.log ] && echo "yes" || echo "no" 	    # 判断文件是否拥有写权限
```

（3）两个文件之间进行比较

```shell
# 测试选项			    作用	
文件1 -nt 文件2	    	判断文件1的修改时间是否比文件2的新（如果新则为真）	
文件1 -ot 文件2	    	判断文件1的修改时间是否比文件2的旧（如果旧则为真）	
文件1 -ef 文件2		    判断文件1是否和文件2的Inode号一致，可以理解为两个文件是否为同一个文件,这个判断用于判断硬链接是很好的方法

# 举例：
ln /root/student.txt /tmp/stu
[ /root/student.txt -ef /tmp/stu ] && echo "yes" || echo "no"
```

（4）两个整数之间比较

```shell
# 测试选项	                	    		作用
整数1 -eq 整数2	    	   判断整数1是否和整数2相等（相等为真）
整数1 -ne 整数2	           判断整数1是否和整数2不相等（不相等为真）
整数1 -gt 整数2            判断整数1是否大于整数2（大于为真）
整数1 -lt 整数2            判断整数1是否小于整数2（小于为真）
整数1 -ge 整数2	           判断整数1是否大于等于整数2（大于等于为真）
整数1 -le 整数2	           判断整数1是否小于等于整数2（小于等于为真）

# 举例：
[ 23 -ge 22 ] && echo "yes" || echo "no" 
```

（5）字符串的判断

```shell
# 测试选项	            		作用
-z 字符串        	      判断字符串是否为空（为空返回真）
-n 字符串	        	  判断字符串是否为非空（非空返回真）
字串1 == 字串2	    	 判断字符串1是否和字符串2相等（相等返回真）
字串1 != 字串2	    	 判断字符串1是否和字符串2不相等（不相等返回真）

举例：
name=fengj
[ -z $name ] && echo "yes" || echo "no"      # 输出no

name=""
[ -z $name ] && echo "yes" || echo "no"      # 输出yes
    	
aa=11
bb=22
[ "$aa" == "$bb" ] && echo "yes" || echo "no" # 判断两个变量的值是否相等，明显不相等，所以返回no
```

（6）多重条件判断

```shell
# 测试选项	            	    		作用
判断1 -a 判断2	    		逻辑与,判断1和判断2都成立,最终的结果才为真					
判断1 -o 判断2	    		逻辑或,判断1和判断2有一个成立,最终的结果就为真
! 判断	                 逻辑非,使原始的判断式取反

举例：
[ "$aa" == "$bb" -a "$aa" -gt 3] && echo "yes" || echo "no"
判断变量aa是否有值,同时判断变量aa是否大于23
```

### 2.6.2 if

（1）单分支if语句

1. 单分支if语句概述

```shell
# 学习小脚本实例的好处：
1 掌握语法结构
2 了解shel的作用
3 分析编程思想

# 建立编程思想的方法：
1 熟悉Linux基本命令、规范、语法及shell语法
2 当遇到实际需求时，应用所学知识

# 如何“背”程序?
1 抄写老师的程序并能正确运行
2 为程序补全注释
3 删掉注释，为代码重新加注释
4 看注释写代码
5 删掉代码和注释，从头开始写
```

2. 单分支if语句

```shell
# (1) 单分支if条件语句
if [ 条件判断式 ];then
	command1
fi

或者

if [ 条件判断式 ]
then
	command1
fi

# (2) 单分支条件语句需要注意几个点
1. if语句使用fi结尾,和一般语言使用大括号结尾不同
2. [条件判断式]就是使用test命令判断,所以中括号和条件判断式之间必须有空格
3. then后面跟符合条件之后执行的程序,可以放在[]之后，用";"分割,也可以换行写入，就不需要";"了

if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi

# (3) 判断登陆的用户命令
whoami

env | grep USER | cut -d "=" -f 2	    # 筛选USER，以分隔符"="进行分割，提取第二个字段
# (4) 例子
例子1：判断登陆的用户是否root
#!/bin/bash 
test=$(env | grep "USER" | cut -d "=" -f 2)   # echo $test
if [ "$test" == root ]
then
	echo "Current user is root."
fi
```

3. 判断分区使用率

```shell
df -h | grep /dev/vda1 | awk '{print $5}' | cut -d "%" -f 1

(1) 例子 - 判断分区使用率
#!/bin/bash
test=$(df -h | grep /dev/vda1 | awk '{print $5}' | cut -d "%" -f 1)
echo $test	        	            # vda1分区使用率
if [ "$test" -ge "90" ]	        	# -ge大于等于90
then
	echo "/ is full!根磁盘已满!"
fi
```

（2）双分支if语句

```shell
# (1) 双分支if条件语句

if [ 条件判断式 ]
then
	条件成立时，执行的程序
else
	条件不成立时，执行另一个程序
fi

# (2) 举例：

#!/bin/bash
read -t 30 -p "Please input a dir:" dir
if [ -d "$dir" ]  # 如果输入的是一个目录
then
	echo "is a directory"
else
	echo "is not a directory"
fi
```

```shell
# (1) 判断apache是否启动
美工 + 程序 +　数据库　＋ apache服务 + linux服务器 + 域名 + IP地址 +　带宽 ＋　网页　＝　网站

www.netcraft.com                           # 网页查看一些服务器数据统计
ps aux | grep httpd | grep -v grep         # 查询httpd是否启动

# (2) 判断apache是否启动 (脚本名称不能含有httpd)
#!/bin/bash 
test=$(ps aux | grep httpd | grep -v grep) # 截取httpd进程，把结果赋给变量test
if [ -n "$test" ]                          # -n非空,执行then中命令
then
	echo "httpd is open!"
else
	echo "httpd is closed!!"
fi
# (3) 定时执行
win 计划任务
```

（3）多分支if语句

```shell
# (1) 语法
if [ 条件判断式1 ]
then
	条件成立时，执行的程序
elif [ 条件判断式2 ]
then
	条件不成立时，执行另一个程序
else
	当所有条件都不成立时，最后执行的程序
fi
# (2) 例子 加减乘除计算器
```



### 2.6.3 case语句

（1）多分支case语句

case工作方式，取值后面必须为单词in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;; ( ;; 表示break，即执行结束 )

取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。

```shell
#(1) 多分支case条件语句
case语句和if...elif...else语句一样都是多分支条件语句,不过和if多分支条件语句不同的是,case语句只能判断一种条件关系,而if语句可以判断多种条件关系。

#(2) 语法
case $变量名 in
	"值1")
		如果变量的值等于值1，则执行程序1
		;;
	"值2")
		如果变量的值等于值2，则执行程序2
		;;
		…省略其他分支…
	*)
		如果变量的值都不是以上的值，则执行此程序
		;;
esac

#(3) 例子

#!/bin/bash
read -t 30 -p "please input yes/no:" choice
case "$choice" in
	"yes")
		echo "yes"
		;;  
	"no")
		echo "no"
		;;  
	*)  
		echo "please input yes/no again!!!"
		;;  
esac
```

### 2.6.3 循环

（1） for循环

```shell
# (1) 语法1：

for 变量 in 值1 值2 值3 …
do
	程序
done

for var in item1 item2 ... itemN; do command1; command2… done;  # 写成一行

# (2) 语法2: 如果要在循环体中进行 for 中的 next 操作，记得变量要加 $，不然程序会变成死循环。
for((assignment;condition;next));do
    command_1;
    command_2;
    commond_..;
done;

# 示例1:
#!/bin/bash 
for i in 1 2 3 4 5
do 
		echo $i
done

# 示例2:
#!/bin/bash
cd /root/test
ls *.tar.gz > ls.log   # 输出重定向,覆盖
ls *.tgz >> ls.log     # 输出重定向,追加
for i in $( cat ls.log )
do  
	tar -czf $i &>/dev/null  # 把执行过程中所有输出信息丢到回收站,不显示在屏幕上
done
rm -rf ls.log

# 实例3:
#!/bin/bash
for((i=1;i<=5;i++));do
    echo "这是第 $i 次调用";
done;
```

（2） while循环：while循环是不定循环，也称作条件循环。只要条件判断式成立,循环就会一直继续,直到条件判断式不成立,循环才会停止。这就和for的固定循环不太一样了。

```shell
#(1) while循环 - 语法
while [ 条件判断式 ]
do
	程序
done

# (2) 例子
#!/bin/bash
i=1
s=0
while [ $i -le 100 ]      # 如果变量i的值小于等于100，则执行循环
do 
	s=$(( $s+$i ))
	i=$(( $i+1 ))
done 
echo "The sum is：$s"
```

（3）until循环

```shell
# (1) 语法
until循环,和while循环相反,until循环时,只要条件判断式不成立则进行循环,并执行循环程序.一旦循环条件成立,则终止循环

until [条件判断式]
do
	程序
done

# (2) 例子	
#!/bin/bash
i=1
s=0
until [ $i -gt 100 ]      # 如果变量i的值大 于等于100，则执行循环
do 
	s=$(( $s+$i ))
	i=$(( $i+1 ))
done 
echo "The sum is：$s"
```

（4）无限循环

```shell
while ;
do
	command
done
# 或者
while true
do
	command
done
# 或者
for (( ; ;))
```

（5）跳出循环

1. break命令

```shell
# 脚本进入死循环直至用户输入数字大于5。要跳出这个循环，返回到shell提示符下，需要使用break命令
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done
```

2. continue命令

continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。 

## 2.7 函数

1. linux shell中的函数和大多数编程语言中的函数一样
2. 将相似的任务和代码封装在函数中，以便于方便调用

### 2.7.1 函数定义和使用

（1）第一种格式

```shell
name()
{
    command1
    command2
    .....
    commandn
}
```

（2）第二种格式

```shell
function name
{
    command1
    command2
    .....
    commandn
}
```

### 2.7.2 向函数传递参数

（1）直接使用函数名调用，可以将其想象成shell中的一条命令

```shell
[root@Nihaowa ~]# test()                    # 定义函数 test
> {
> echo "test function"
> }
[root@Nihaowa ~]# test                      # 调用函数 test
test function
```

（2）案例

```shell
# 需求描述：写一个监控nginx的脚步：如果nginx服务宕机，则该脚本可以检测到并将进程启动
#!/bin/bash
this_pid==$$                                       # 运行脚本生成子进程的pid
while true
do
ps -ef | grep nginx | grep -v grep &> /dev/null    # 丢垃圾桶
    if [ $? -eq 0 ];then
        echo "Nginx is running well"
        sleep 3
    else
        systemctl start nginx
        echo "Nginx is down, Start it ...."
    fi
done
```

（3）`调用函数 function_name $1...`, 函数内部可以直接使用参数 $1  ... 

1. 高级语言函数定义及调用

```c
int example_1(int arg1, int arg2)
{
    arg1=arg2;
    .....
    return null;
}
```

```c
int num1=10;
int num2=20;
int num3=example_1(num1, num2);                   # 函数调用
```

2. shell中传参

```
function name
{
    echo "Hello $1"
    echo "Hello $2"
}
```

（4）案例

```shell
# 写一个脚本，该脚本可以实现计算器的功能，可以实现加减乘除
#!/bin/bash
function calculate
{
    case "$2" in
        +)
            echo "`expr $1 + $3`"
            ;;
        -)
            echo "`expr $1 - $3`"
            ;;
        x)
            echo "`expr $1 \* $3`"
            ;;
        /)
            echo "`expr $1 / $3`"
            ;;
    esac
}
calculate $1 $2 $3
```

```shell
[root@Nihaowa ~]# sh example_6_calcu.sh 12 x 56
672
```

### 2.7.3 函数返回值

（1）函数返回值的方式

```
方法一: return
方法二: echo
```

（2）使用return返回值

1. 使用return返回值，只能返回1-255的整数
2. 使用return返回值，通常只是用来供其他地方调用获取状态，因此通常仅返回0或1；0表示成功，1表示失败

（3）案例：判断nginx进程状态

```shell
#!/bin/bash
this_pid=$$
function is_nginx_running
{
    ps -ef | grep nginx | grep -v grep &> /dev/null
    if [ $? -eq 0 ];then
        return
    else
        return 1
    fi
}
is_nginx_running && echo "Nginx is running" || echo "Nginx is stoped"
```

（4）使用echo返回值

1. 使用echo可以返回任何字符串结果
2. 通常用于返回数据，比如一个字符串值或者列表值

（5）案例

```shell
#!/bin/bash
function get_users
{
    users=`cat /etc/passwd | cut -d: -f1`
    echo $users
}

user_list=`get_users`

index=1
for u in $user_list
do
    echo "This $index user is: $u"
    index=$(($index+1))
done
```



### 2.7.4 局部变量和全局变量

（1）全局变量

1. 不做特殊声明，shell中变量都是全局变量
2. 大型脚本程序中慎用全局变量

（2）局部变量

1. 定义变量时，使用local关键字
2. 函数内部和外若存在同名变量，则函数内部变量覆盖外部变量

3. 在函数内部定义的变量，在函数调用后，就成为一个全局变量

### 2.7.5 函数库

（1）为什么需要定义函数库

1. 经常使用的重复代码封装成函数文件
2. 一般不直接执行，而是由其他脚本调用

（2）经验之谈

1. 库文件名的后缀是任意的，但一般使用 `.lib`
2. 库文件通常没有可执行选项
3. 库文件无需和脚本在同级目录，只需在脚本中引用时指定
4. 第一行一般使用 `#!/bin/echo`，输出警告信息，避免用户执行

（3）案例

```shell
#!/bin/bash

. /root/workspace/backup/shell/lib/base_function          # 调用函数库

add 12 23
reduce 90 30
```

```shell
# 定义函数库
# /root/workspace/backup/shell/lib/base_function 库文件
function add
{
    echo "`expr $1 + $2`"
}

function reduce
{
    echo "`expr $1 - $2`"
}
```

# 3 工具函数

## 3.1 字符截取

### 3.1.1 cut 字符截取

1. cut 使用

```shell
grep "/bin/bash" /etc/passwd | grep -v root     # 提取普通用户, grep -v 取反

grep "/bin/bash" /etc/passwd | grep -v root | cut -f 1 -d ":"   # 提取第一列
user1
user2
```

```shell
cut [选项] 文件名    
选项: -f列名:提取第几列,分隔符默认是 TAB(制表符)
     -d分隔符:按照指定分隔符分隔列
```

2. cut的局限性

```shell
# df 查看系统分区的使用状况, -h人性化显示
df -h | cut -d "" -f 1,3                 # 截取不到第三列
```

（2）printf  '输出类型输出格式'  输出内容 

1. 输出类型与输出格式

```shell
# 输出类型
%ns: 输出字符串,n是数字指代输出几个字符
%ni: 输出整数,n是数字指代输出几个数字
%m.nf: 输出浮点数,m和n是数字,指代输出的整数位数和小数位数,如 %8.2f代表共输出8位数,其中2位是小数,6位是整数
# 输出格式:
\a: 输出警告声音
\b: 输出退格键,也就是Backspace键
\f: 清除屏幕
\n: 换行
\r： 回车,也就是Enter键
\t: 水平输出退格键,也就是Tab键
\v: 垂直输出退格键,也就是Tab键
```

```shell
[root@Nihaowa ~]# printf %s 1 2 3 4 6
12346[root@Nihaowa ~]# printf 1 2 3 4 6
1[root@Nihaowa ~]#

[root@Nihaowa ~]# printf %s %s %s 1 2 3 4 6
%s%s12346[root@Nihaowa ~]#

# 先输出三个字符串 1 2 3,再输出三个字符串4 5 6
%s%s12346[root@Nihaowa ~]# printf '%s %s %s' 1 2 3 4 5 6
1 2 34 5 6[root@Nihaowa ~]#

[root@Nihaowa ~]# printf '%s\t%s\t%s\n' 1 2 3 4 5 6
1       2       3
4       5       6

# 不调整输出格式
[root@Nihaowa ~]# printf '%s' $(cat /etc/passwd)
```

2. 在awk命令的输出中支持print和printf命令

* print：会在每个输出之后自动加入一个换行符（Linux默认没有print命令，awk命令的一个子命令）
* printf：是标准格式输出命令，并不会自动加入换行符，如果需要换行，需要手动加入换行符

### 3.1.2 awk

（1）awk 命令

1. awk命令格式

* awk默认使用空格或者制表符作为分隔符

```
# awk '条件1{动作1}条件2{动作2}...' 文件名
# 条件(pattern)
	一般使用关系表达式作为条件
	x > 10 判断变量x是否大于10
	x >= 10 大于等于
# 动作(Action)
	格式化输出
	流程控制语句
```

```shell
[root@Nihaowa ~]# df -h | awk '{printf $1 "\t" $3}'
Filesystem      Useddevtmpfs    0tmpfs  24Ktmpfs        548Ktmpfs       0/dev/vda1      10Gtmpfs0[root@Nihaowa ~]# df -h | awk '{print $1 "\t" $3}'
Filesystem      Used
devtmpfs        0
tmpfs   24K
tmpfs   548K
tmpfs   0
/dev/vda1       10G
tmpfs   0
```

2. BEGIN

```shell
[root@Nihaowa ~]# df -h | awk 'BEGIN{print "Nihaowa"}{print $1 "\t" $3}'
Nihaowa
Filesystem      Used
devtmpfs        0
tmpfs   24K
tmpfs   548K
tmpfs   0
/dev/vda1       10G
tmpfs   0
```

3. END

```shell
[root@Nihaowa ~]# df -h | awk 'END{print "Nihaowa"}{print $1 "\t" $3}'
Filesystem      Used
devtmpfs        0
tmpfs   24K
tmpfs   548K
tmpfs   0
/dev/vda1       10G
tmpfs   0
Nihaowa
```

4. FS内置变量

```shell
# 对于awk而言,先把第一行数据读取进来,将它赋予 $1 变量之后,执行 {printf $1 "\t" $3 "\n"} 这个动作,{FS=":"}这个动作,第一行数据并没有执行

[root@Nihaowa ~]# cat /etc/passwd | grep "/bin/bash" | awk '{FS=":"}{printf $1 "\t" $3 "\n"}'
user1:x:1000:1000::/home/user1:/bin/bash
user2   1001

# 先执行这个动作: BEGIN{FS=":"}
[root@Nihaowa ~]# cat /etc/passwd | grep "/bin/bash" | awk 'BEGIN{FS=":"}{printf $1 "\t" $3 "\n"}'
user1   1000
user2   1001

```

5. 关系运算符

![awk](pic\57-awk.png)

6. awk执行原理

* 由于awk的执行原理是先读取数据再判断条件然后执行动作；所以awk在不加BEGIN的情况下先读取第一行数据也就是`"root:0:0:root:/root:/bin/bash"`再执行`{FS=":"}`、`{printf $1 "\t" $3 "\n"}`这两个动作，而此时由于第一条数据已经读取，因此`{FS=":"}` 这一动作只对后两条数据起作用 

### 3.1.3  sed

1. 定义：sed命令

   sed是一种几乎包括在所有UNIX平台（包括Linux）的轻量级编辑器。sed主要是用来将数据进行选取、替换、删除、新增的命令

2. 格式

```shell
sed [选项] '[动作]' 文件名
选项：
	-n  一般sed命令会把所有数据都输出到屏幕，如果加入此选项则只会把经过sed命令处理过的行到屏幕 
    -e  允许一次应用多个动作 
    -i	直接修改文件内容,并且不由屏幕输出  
动作: 
    a	追加,在当前行后添加一行或多行 
    c	行替换，用c后面的字符串替换原数据行 
    i	插入,在当前行前插入一行或多行 
    d   删除指定行
    p   打印,输出指定行
    s   字串替换，用一个字符串替换一个字符串(替换格式与vim中的类似)。格式为"行范围s/旧字串/新字串/g"

举例：
    sed '2p' student.txt	        # 会把第二行显示，然后将所有内容显示
    sed -n '2p' student.txt	    	# 查看文件的第二行
    
    sed '2,4d' student.txt		    # 删除第二行到第四行，删除的只是显示的值并不改变文件内容
    
    sed '2a piaoliang jiu shi ren xing' student.txt    		# 在第二行追加字符
    sed '2i piaoliang jiu shi ren xing' student.txt	    	# 在第二行插入字符
    
    sed '4c cang bu ji ge' student.txt	    # 替换第四行
    
    sed '4s/70/100/g' student.txt		    # 第四行70替换成100，/g代表所有都替换
    sed -i '4s/70/100/g' student.txt	    # 文件内容70被替换成100
    sed -e 's/furong//g;s/fengj//g' student.txt	# 应用多个动作，用;隔开,/g代表所有都替换
```

## 3.2  字符处理

### 3.2.1 sort

```shell
排序命令sort
sort [选项] 文件名
选项：
    -f	      忽略大小写 
    -n        以数值型进行排序,默认使用字符串型排序 
	-r        反向排序 
	-t        指定分隔符,默认是制表符 
	-k n[,m]  按照指定的字段范围排序.从第n字段开始,m字段结束(不加m默认到行尾) 
举例：
    sort /etc/passwd	    # 按字母顺序排列
    sort -r /etc/passwd	    # 取反按字母顺序排列
    sort -n -t ":" -k "3,3" /etc/passwd		
    # 指定分隔符是":"，用第三字段开头，第三字段结尾排序，也就是只用第三字段排序，-n代表数值排序
```

### 3.2.2 wc

```
统计命令wc
	wc [选项] 文件名 
选项：
    -l	      只统计行数 
    -w	      只统计单词数 
    -m	      只统计字符数包括开空格
```

## 3.3 字符查找

### 3.3.1 find

（1）find命令

| 选项            | 含义                       |
| --------------- | -------------------------- |
| -name           | 根据文件名查找             |
| -perm           | 根据文件权限查找           |
| -prune          | 该选项可以排除某些查找目录 |
| -user           | 根据文件属主查找           |
| -group          | 根据文件属组查找           |
| -mtime -n \| +n | 根据文件更改时间查找       |

| 选项                 | 含义                                    |
| -------------------- | --------------------------------------- |
| -nogroup             | 查找无有效属组的文件                    |
| -nouser              | 查找无有效属主的文件                    |
| -newer file1 ! file2 | 查找更改时间比file1新但比file2旧IDE文件 |
| -type                | 按文件类型查找                          |
| -size -n +n          | 按文件大小查找                          |
| -mindepth n          | 从n级子目录开始搜索                     |
| -maxdepth n          | 最多搜索到n级子目录                     |

```shell
find /etc -name '*.conf'                # 根据文件名称查找,查找/etc 目录下以conf结尾的文件
/etc/audit/auditd.conf
/etc/sysctl.conf

# 查找当前目录下文件名为aa的文件,不区分大小写   find . -name aa
# 查找当前目录下文件属主为hdfs的所有文件       find . -user hdfs
# 查找当前目录下文件属组为yarn的所有文件       find . -group yarn
# 按文件类型查找           -type
f   文件                                 find . -type f
d   目录                                 find . -type d
c   字符设备文件                           find . -type c
b   块设备文件                            find . -type b
l   链接文件                              find . -type l
p   管道文件                              find . -type p

# 按文件大小查找            -size
-n  大小小于n的文件
+n  大小大于n的文件
# 查找/etc 目录下小于10000字节的文件         find /etc -size -10000c
# 查找/etc 目录下大于1M的文件               find /etc -size +1M

# 按修改时间(天)查找         -mtime
-n  n天以内修改的文件
+n  n天以外修改的文件
n   n正好n天修改的文件

# 查找/etc 目录下5天以内修改且以conf结尾的文件
find /etc -mtime -5 -name '*.conf'
# 查找/etc 目录下10天之前修改且属主为root的文件
find /etc -mtime +10 -user root

# 按修改时间(分钟)查找        -mmin
-n  n分钟以内修改的文件
+n  n分钟以外修改的文件
# 查找/etc目录下30分钟之前修改的文件
find /etc -mmin +30
# 查找/etc目录下30分钟之内修改的目录
find /etc -mmin -30 -type d

# 表示从n级子目录开始搜索       -mindepth n
# 在/etc下的3级子目录开始搜索
find /etc -mindepth 3

# 表示最多搜索到n级子目录       -maxdepth n
# 在/etc 下搜索符合条件的文件，但最多搜索到2级子目录
find /etc -maxdepth 2 -name '*.conf'
```

2. 其他命令总结

```shell
dd if=/dev/zero of 123 bs=512k count=2
```

3. find其他选项

```shell
-nouser    查找没有属主的用户       例: find . -type f -nouser
-nogroup   查找没有属组的用户       例: find . -type f -nogroup
-perm      查找权限为某个值的用户    例: find . -perm 664
-prune     该选项可以排除某些查找目录
```

（2）对查找的文件进行操作

```shell
-print            打印输出

-exec             对搜索的文件执行特定的操作,格式为 -exec 'command' {} \;

例子1: 搜索/etc 下的文件(非目录)，文件名以conf结尾，且大于10k,然后将其删除
find /etc/ -type f -name '*.conf' -size +10k -exec rm -f {} \;

例子2: 将/var/log/ 目录下以log结尾的文件，且更改时间在7天以上的删除
find /var/log/ -name '*.log' -mtime +7 -exec rm -rf {} \;

例子3: 搜索条件和例1一样，只是不删除，而是将其复制到/root/conf 目录下
find /etc/ -type f -name '*.conf' -size +10k -exec cp {} /root/conf \;

-ok                和exec功能一样,只是每次操作都会给用户提示
```



### 3.3.2 locate、whereis和which总结

（1）locate

1. 文件查找命令，所属软件包mlocate
2. 不同于find命令是在整块磁盘中搜索，locate命令在数据库文件中查找

```shell
# updatedb命令 mlocate.db 数据库
# 用户更新 /var/lib/mlocate/mlocate.db 
系统定时任务，将系统中的文件定时更新到这个数据库中

# 所使用配置文件 /etc/updatedb.conf
# 该命令在后台cron计划任务中定期执行
```

3. find是默认全部匹配，locate则是默认部分匹配

```shell
[root@Nihaowa shell]# find /etc -name 'my.cnf'         # 默认全部匹配
/etc/my.cnf
[root@Nihaowa shell]# locate my.cnf                    # 默认部分匹配
/etc/my.cnf
/etc/my.cnf.d
```

（2）whereis

1. 查找二进制程序文件，及帮助文档文件、源代码文件

| 选项 | 含义               |
| ---- | ------------------ |
| -b   | 只返回二进制文件   |
| -m   | 只返回帮助文档文件 |
| -s   | 只返回源代码文件   |

```shell
[root@Nihaowa shell]# whereis mysql
mysql: /usr/bin/mysql /usr/lib64/mysql /usr/share/mysql /usr/share/man/man1/mysql.1.gz
```

（3）which

1. 作用：仅查找二进制程序文件

| 选项 | 含义             |
| ---- | ---------------- |
| -b   | 只返回二进制文件 |

（4）各命令使用场景推荐

| 命令    | 适用场景                           | 优缺点                   |
| ------- | ---------------------------------- | ------------------------ |
| find    | 查找某一类文件，比如文件名部分一致 | 功能强大，速度慢         |
| locate  | 只能查找单个文件                   | 功能单一，速度快         |
| whereis | 查找程序的可执行文件、帮助文档等   | 不常用                   |
| which   | 只查找程序的可执行文件             | 常用于查找程序的绝对路径 |



