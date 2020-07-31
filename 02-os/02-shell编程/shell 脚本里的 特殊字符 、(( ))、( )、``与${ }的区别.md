# [shell 脚本里的 特殊字符 $(( ))、$( )、``与${ }的区别](https://www.cnblogs.com/chenpython123/p/11052276.html)

### 1. 在bash中，`$( )`与`` ``（反引号）都是用来作命令替换的。

　　**命令替换**与变量替换差不多，都是用来重组命令行的，先完成引号里的命令行，然后将其结果替换出来，再重组成新的命令行。

1. $( ) 与 ｀｀

* 在操作上，这两者都是达到相应的效果，但是建议使用$( )，理由如下：

* ` ｀｀`很容易与` ' ' `搞混乱，尤其对初学者来说，而`$( ) `比较直观。最后，`$( )`的弊端是，并不是所有的类unix系统都支持这种方式，但反引号是肯定支持的。



### 2. `${ } `变量替换 、 ${} 里面还可以有  #*,##*,#*,##*,% *,%% * 

　　1. 一般情况下，$var与${var}是没有区别的，但是用${ }会比较精确的界定变量名称的范围。
  　　2. exp 1

```
[root@localhost ~]# A=Linux
[root@localhost ~]# echo $AB      # 表示变量AB

[root@localhost ~]# echo ${A}B    #表示变量A后连接着B
LinuxB
```



### 3. $(( )) 可以 整数运算、进制转换、重定义变量值

1. bash中整数运算符号

| 符号     | 功能                      |
| -------- | ------------------------- |
| + - * /  | 分别为加、减、乘、除      |
| %        | 余数运算                  |
| & \| ^ ! | 分别为“AND、OR、XOR、NOT” |



```
[root@localhost ~]# echo $((2*3))
6
[root@localhost ~]# a=5;b=7;c=2
[root@localhost ~]# echo $((a+b*c))
19
[root@localhost ~]# echo $(($a+$b*$c))
19
```

2. `$(( )) `可以将其他进制转成十进制数显示出来。用法如下：`echo $((N#xx))`
   其中，N为进制，xx为该进制下某个数值，命令执行后可以得到该进制数转成十进制后的值。

```
[root@localhost ~]# echo $((2#110))
6
[root@localhost ~]# echo $((16#2a))
42
[root@localhost ~]# echo $((8#11))
9
```

4. ` (( )) `重定义变量值，是 [ ] 数学表达式的加强版

```
[root@localhost ~]# a=5;b=7
[root@localhost ~]# ((a++))
[root@localhost ~]# echo $a
6
[root@localhost ~]# ((a--));echo $a
5
[root@localhost ~]# ((a<b));echo $?
0
[root@localhost ~]# ((a>b));echo $?
1
```

### 4. `${name[*]}`  ` ${name[@]} ` ` ${#name[*]} ` 区别

1. 在linux的shell里，${name}可以表示变量，也可以表示数组。
2. name后面加 [ ] 的，一般是数组，
3. `${name[*]} `   是数组所有元素(all of the elements)
4. `${name[@]} `  是数组每一个元素(each of the elements)  ，循环数组用这个 。

5. `${#name[*]}`  是数组元素的个数，也可以写成  `${#name[@]} `

6. `${name:-Hello}` 是指，如果`${name}`没有赋值，那么它等于Hello,如果赋值了，就保持原值，不用管问这个Hello了。

7. `${!array_name[@]} `  、`${!array_name[*]} `  获取数组的下标。



### 5. **单小括号 ()**







　　　