# 1 Python介绍与安装

课程资料 https://github.com/wilsonyin123/geekbangpython

## 1.1 特点

（1）语法简洁

（2）跨平台

（3）可扩展（可扩充C++语言库）

（4）开放源码

（5）类库丰富（图形图像处理、生物库、科学计算）

## 1.2 发展历史及版本

（1）发展

1. 1990年诞生 ---> 2000 Python2.0发布 ---> 2008年 Python3.0发布  ---> 2010年 Python2.7发布

2. Python2.x 已经是遗产，Python3.x 是现在和未来的语言

（2）版本

1. Python官方版（解释器+标准库）与发行版（解释器+标准库+常用软件包）

2. Python最流行发行版 ：Anaconda

（3）常用工具

1. python官方文档
2. iPython（命令补全）
3. jupyter notebook
4. sublime text（文本编辑器）
5. PyCharm（IDE集成开发环境）
6. Pip（解决第三方库的依赖关系问题）

## 1.3 Python安装



# 2 变量与数据结构

## 2.1 程序书写规则

（1）具有相同缩进的代码被视为代码块。缩进在Python中具有严格的习惯写法：**4个空格**，**不要**使用Tab
 if语句后接表达式，然后用 : 表示代码块的开始。 

（2）有意义的缩进 与Yaml类似

（3）语句不需要以 分号结尾，与shell类型，C/C++语言语句需要以分号结尾

（4）字符串什么时候用单引号或双引号？ 字符串中已包括了单引号，这个时候就要改用双引号

```python
chinesezodiac = " that's xxx"
```

（5）shell中单引号与双引号 [单引号与双引号参考链接](https://blog.csdn.net/LuckyQueen0928/article/details/86608305)

1. 首先说下他们的共同点: 好像就只有一个，就是它们都可以用来界定一个字符串，这个没什么好解释的，真正需要记住的是它们区别
2. 它们的区别主要包括：

```shell
(1) 单引号属于强引用,它会忽略所有被引起来的字符的特殊处理,被引用起来的字符会被原封不动的使用,唯一需要注意的点是不允许引用自身
(2) 双引号属于弱引用,它会对一些被引起来的字符进行特殊处理,主要包括以下情况： 

1: $加变量名可以取变量的值,比如
[root@localhost ~]# echo '$PWD'
$PWD　　

[root@localhost ~]# echo "$PWD"
/root 

2: 反引号和$()引起来的字符会被当做命令执行后替换原来的字符，比如
[root@localhost ~]# echo '$(echo hello world)'
$(echo hello world)
[root@localhost ~]# echo "$(echo hello world)"
hello world

[root@localhost ~]# echo '`echo hello world`'
`echo hello world`
[root@localhost ~]# echo "`echo hello world`"
hello world

3: 当需要使用字符（$  `  "  \）时必须进行转义，也就是在前面加 \
[root@localhost ~]# echo '$ ` " \'
$ ` " \
[root@localhost ~]# echo "\$ \` \" \\"
$ ` " \
```



## 2.2 基础数据类型

（1）整数（int）

（2）浮点数（float）

（3）字符串（str）

（4）布尔值（bool）：True、False

```python
>>> bool(123)
True
>>> bool(0)
False
```

```python
>>> type(8.3)                     # python: type()  C/C++: typeid(val).name()
<class 'float'>
>>> type("字符串")                 # C语言没有字符串类型 char *p = "linux";
<class 'str'>                     # python字符串类型str与C++标准库字符串类 std::string类似
>>> int('8')                      # python类型转换与C/C++类似
8
```

## 2.3 变量定义和常用操作

（1）变量命名规则

（2）变量命名规范：驼峰（大驼峰，小驼峰）

```python
message = "Hello Python World"
message = "Hello Python Crash course World"       # python允许变量同名
print(message)
```

## 2.4 序列

（1）序列：是指它的成员都是有序排列，并且可以通过下标偏移量访问到它的一个或几个成员

（2）分类：字符串、列表、元组三种类型

```
字符串  "abcdef"
列表    [12345, "abcde"]      # 存储的内容可变更
元组    ("abc", "def")        # 存储的内容一般不可变更
```

（3）序列的基本操作

1. 成员关系操作符 （  in、 not in  ）
2. 连接操作符（  +  ）
3. 重复操作符（  *  ）
4. 切片操作符（  [:]  ）

## 2.5 字符串

（1）python中对于字符串的操作

```python
chineseZodiac = "鼠牛虎兔龙蛇马羊猴鸡狗猪"
print(chineseZodiac[0:4])
print(chineseZodiac[-1])
```

（2）对比C++字符串操作`  std::string::substr `

```

```

（3）字符串操作

```python
chineseZodiac = "猴鸡狗猪鼠牛虎兔龙蛇马羊"
print('狗' not in chineseZodiac)                # False
print(chineseZodiac + chineseZodiac)            # 猴鸡狗猪鼠牛虎兔龙蛇马羊猴鸡狗猪鼠牛虎兔龙蛇马羊
print(chineseZodiac * 2)
```

## 2.6 元组

（1）元组定义

```python
Zodiac_name = ( u'摩羯', u'宝瓶', u'双鱼', u'白羊', u'金牛', u'双子',
                u'巨蟹', u'狮子', u'室女', u'天秤', u'天蝎', u'人马')
Zodiac_days = ((1, 20), (2, 19), (3, 21), (4, 21), (5, 21), (6, 22),
               (7, 23), (8, 23), (9, 23), (10, 23), (11, 23), (12, 23))

(mounth ,day) = (2, 15)
zodic_day = filter(lambda x: x <=(mounth, day), Zodiac_days)
print(Zodiac_name[len(list(zodic_day))])
```

```python
>>> a=(1, 3, 5, 7)
>>> b=4
>>> filter(lambda x: x < b, a)               # filter与lambda表达式
<filter object at 0x00938160>
>>> list (filter(lambda x: x < b, a))        # 取出a中小于4的结果  list使用
[1, 3]
>>> len (list (filter(lambda x: x < b, a)))  # 取出a中小于4的元素个数  len使用
2
```

（2）元组大小比较

```python
>>> (4) > (5)
False
>>> (1, 4) > (2, 1)                         # 元组比较大小相当于：数字叠加比较 14 > 21
False
```

（3）对比C++ tuple

```c++

```

## 2.7 列表

（1）列表添加元素、移除元素

```python
a_list = ['abc', 'xyz']
a_list.append('efg')
a_list.remove('xyz')
print(a_list)
```

## 2.8 映射的类型：字典

（1）字典：字典包含哈希值和指向的对象 

```python
{"哈希值" : "对象"}
dict = {'x':1, 'y':2}
dict['z'] = 3
print(dict)

>>> type({})
<class 'dict'>
```

（2）

# 3 条件判断循环

## 3.1 条件语句

（1）python if 语法

```python
if 表达式:
    代码块
```

````python
if 表达式:
    代码块
elif 表达式:
    代码块
else:
    代码块
````

```python
x = 'abcd'
if x == 'abc':
    print('x和abcd相等')
else:
    print('x和abcd不相等')
```

```python
year = int(input('请用户输入出生年份'))     # python输入input, shell输入是read,C++是std::cout
if (chineseZodiac[year % 12]) == '鼠':
    print('xxxxxxx')
```

（2）shell if 语法

```shell
if condition
then
    command1 
    ...
    commandN 
fi
```

1. shell 条件判断 if 与python条件判断的区别：

## 3.2 循环

 （1）for

```python
for 迭代变量 in 可迭代对象:
    代码块
```

```python
for i in range(1, 13):
    print(i)
    
for year in range(2015, 2020):                      # range范围
    print('%s 年的生肖是 %s' %(year, chineseZodiac[year % 12]))  # python变量替换
```

（2）while

```python
while 表达式:
    代码块
```

（3）continue与break使用

（4）shell for与while语法

```shell
for var in item1 item2 ... itemN
do
    command1
    ...
    commandN
done
```

```shell
while condition
do
    command
done
```

## 3.3 循环与判断

（1）for循环语句中的 if 嵌套

```python
month = (int)(input('请输入月份: '))
day = (int)(input('请输入日期: '))
for zd_num in range(len(Zodiac_days)):
    if Zodiac_days[zd_num] >= (month, day):
        print(Zodiac_name[zd_num])
        break
    elif month == 12 and day > 23:
        print(Zodiac_name[0])
        break
```

（2）while循环中if嵌套

```python
n = 0
while Zodiac_days[n] < (month, day):
    if month == 12 and day > 23:
        break
    n += 1
print(Zodiac_name[n])                     # 注意此处print要与while对齐     
```

# 4 文件、输入输出、异常

## 4.1 



##

# 5 函数、模块



# 6 面向对象编程





# 7 多线程编程



# 8 标准库





# 9 第三方库



# 10 综合项目