今天写bash脚本,运行时报$'\r': command not found错误,经过查找,原来是windows和Linux的换行符不同(windows是\r\n,而Linux是\n)导致的,找了好久答案:

有说装dos2unix的,失败原因是找不到软件源,最终放弃.

有的说vim的命令行模式下使用%s/^M//g的,没效果,放弃.

最终找到了解决方法:https://blog.csdn.net/baidu_36649389/article/details/75339010,感谢!

如下:

# vi filename

命令行模式下,输入:

:set ff=unix 将换行符设置成UNIX的模式

(知识扩展:https://wenda.so.com/q/1534876411219608)

:wq 退出

即可解决这个换行符的问题.
————————————————




# 1 grep和egrep

1. grep 过滤器

2. sed 流编辑器

3. awk 报告生成器

## 1.1 grep

### 1.1.1 grep语法格式

1. 第一种格式：在文件中查找

```shell
# pattern 匹配模式:固定字符串或正则表达式
grep [option] [pattern] [file1, file2 ....]
```

2. 第二种形式：标准输出中查找

```shell
command | grep [option] [pattern]
```

### 1.1.2 grep参数

1. 常用参数

| 选项        | 含义                                                        |
| ----------- | ----------------------------------------------------------- |
| -v          | 不显示匹配行信息                                            |
| -i   ignore | 搜索时忽略大小写                                            |
| -n  number  | 显示行号                                                    |
| -r          | 递归搜索                                                    |
| -E  extern  | 支持扩展正则表达式(基本正则表达式 扩展正则表达式 例如：\| ) |
| -F          | 不按正则表达式匹配，按照字符串字面意思匹配                  |

2. 不常用参数

| 选项       | 含义                                     |
| ---------- | ---------------------------------------- |
| -c   count | 不显示具体内容，只输出匹配行的数量       |
| -w   word  | 匹配整词                                 |
| -x         | 匹配整行                                 |
| -l         | 只显示匹配的文件名，不显示具体匹配行内容 |
| -s         | 不显示错误信息                           |

```shell
grep -E "python|PYTHON" file                # 扩展正则表达式 | 
grep -F "py.*" file                         # 匹配 py.* 字符串
grep -r love                                # 递归搜索所有文件
```

## 1.2 egrep

1. grep默认不支持扩展正则表达式，只支持基础正则表达式
2. 使用grep -E可以支持扩展正则表达式
3. 使用egrep可以支持扩展正则表达式，与grep -E等价

# 2 sed

## 2.1 sed工作模式

1. sed流编辑器。对标准输出或文件逐行进行处理

2. 语法格式

```
第一种形式：stdout | sed [option] "pattern command"
第二种形式：sed [option] "pattern command" file
```

3. sed的处理过程

<img src="pic\07-sed处理过程.png" alt="sed处理过程 " style="zoom:67%;" />



* "pattern command" 在当前行中，进行pattern 匹配，按行搜索，处理，执行command



## 2.2 sed选项

1. sed选项

| 选项 | 含义                               |
| ---- | ---------------------------------- |
| -n   | 只打印模式匹配行                   |
| -e   | 直接在命令行进行sed编辑，默认选项  |
| -f   | 编辑动作保存在文件中，指定文件执行 |
| -r   | 支持扩展正则表达式                 |
| -i   | 直接修改文件内容                   |

```shell
# 对文本sed.txt中的每一行都执行 p(print)操作,原行信息打印，匹配行信息打印，每一行输出两遍
sed '/python/n' sed.txt
i love python                                    # 原行
i love python                                    # 匹配行
I love PYTHON                                    # 原行
Hadoop is bigdata frame                          # 原行

sed -n '/python/p' sed.txt
i love python                                    # 匹配行

sed -n -e '/python/p' -e '/PYTHON/p' sed.txt     # 多个编辑命令
i love python
I love PYTHON

sed -n  sed.txt -f edit.sed

sed -n -r '/python|PYTHON/p'  sed.txt            # -r 支持扩展表达式
i love python
I love PYTHON

sed -n 's/love/like/g;p' sed.txt        # sed.txt文件中的love变为like，打印输出
i like python
I like PYTHON
Hadoop is bigdata frame
 
sed -i 's/love/like/g' sed.txt               # 将sed.txt文件中的love变为like
```



## 2.3 sed中pattern详解

| 匹配模式                      | 含义                                           |
| ----------------------------- | ---------------------------------------------- |
| 10command                     | 匹配到第10行                                   |
| 10,20command                  | 匹配从第10行开始，到第20行结束                 |
| 10,+5command                  | 匹配从第10行开始，到第16行结束                 |
| /pattern1/command             | 匹配到pattern1的行                             |
| /pattern1/, /pattern2/command | 匹配到pattern1的行开始，到匹配pattern2的行结束 |
| 10,/pattern1/command          | 匹配从第10行开始，到匹配到pattern1的行结束     |
| /pattern1/,10command          | 匹配到pattern1的行开始，到第10行匹配结束       |



```shell
1. lineNumber         -- 直接指定行号 打印file文件的第17行
sed -n '17p' /etc/passwd
libstoragemgmt:x:998:997:daemon account for ....

2. startLine, EndLine -- 指定起始行号和结束行号
sed -n '17,20p' /etc/passwd                  # 打印17行 - 20行

3. startLine,+N       -- 指定起始行号，到后面N行
sed -n '5, +3p' /etc/passwd                  # 打印第五行，加三行

4. /pattern1/         -- 正则表达式匹配的行
sed -n '/\/sbin\/nologin/p' /etc/passwd      # 打印不能登录的行
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin

sed -n '/^root/p' /etc/passwd                # 打印以root开头的行
root:x:0:0:root:/root:/bin/zsh

5. /pattern1/,/pattern2/    # 从匹配到pattern1的行，直到匹配到pattern2的行
sed -n '/^nginx/,/^nologin/p' /etc/passwd    # 打印nginx开头,nologin结束的行
nginx:x:995:993:Nginx web server:/var/lib/nginx:/sbin/nologin

6. LineNumber,/pattern1/                     # 从指定行号匹配，直到hdfs开头的行 
sed -n '4,/^hdfs/p' /etc/passwd 

7. /pattern1/,LineNumber                    # 从pattern1匹配行，直到匹配到指定行
```

## 2.4 sed 中编辑命令详解

| 类别 | 编辑命令 | 含义                                   |
| ---- | -------- | -------------------------------------- |
| 查询 | p        | 打印                                   |
| 增加 | a        | 匹配到的行后追加内容                   |
| 增加 | i        | 匹配到的行前追加内容                   |
| 增加 | r        | 将后面指定文件的内容追加到匹配的行后面 |
| 增加 | w        | 将匹配的行内容另存到其他文件中         |

| 类别 | 编辑命令     | 含义                               |
| ---- | ------------ | ---------------------------------- |
| 删除 | d            | 删除                               |
| 修改 | s/old/new    | 将行内第一个old替换为new           |
| 修改 | s/old/new/g  | 将行内全部的old替换为new           |
| 修改 | s/old/new/2g | 将行内2个old替换为new              |
| 修改 | s/old/new/ig | 将行内old全部替换为new，忽略大小写 |

```shell
# 删除第一行
sed -i '1d' passwd
# 删除第一行至第三行
sed -i '1,3d' passwd
# 删除不能登录的行
sed -i '/\/sbin\/nologin/d' passwd
# 删除以root开头,至sync开头的行
sed -i /^root/,/^sync/d passwd
```

```shell
# /bin/zsh 追加This is user which can login to system
sed -i '/\/bin\/zsh/a This is user which can login to system' passwd

cat passwd
root:x:0:0:root:/root:/bin/zsh
This is user which can login to system

# 以root开头的行, adm开头的行，前边追加This is user which can login to system
sed -i '/^root/,/^adm/i This is user which can login to system' passwd

# 将list文件的内容追加到含有 root的行的后面
sed -i '/root/r list' passwd
```

```shell
# 匹配/sbin/nologin 输出到/tmp/user_login.txt文件中
sed '/\/sbin\/nologin/w /tmp/user_login.txt' passwd
```





## 2.5 利用sed查找文件内容







## 2.6 利用sed删除文件内容







## 2.7 利用sed修改文件内容







## 2.8 利用sed追加文件内容



# 3 awk

## 3.1 awk工作模式





## 3.2 awk的内置变量



## 3.3 awk格式化输出之printf



## 3.4 awk模式匹配的两种用法



## 3.5 awk中表达式的用法





## 3.6 awk动作中的条件及循环语句



## 3.7 awk动作中的条件及循环语句



## 3.8 awk中字符串函数



## 3.9 awk中常用选项



## 3.10 awk中数组用法



## 3.11 一个复杂的awk处理生产数据的例子