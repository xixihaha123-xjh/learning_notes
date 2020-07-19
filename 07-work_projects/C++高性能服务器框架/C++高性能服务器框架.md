# C++高性能服务器框架

sylar网站地址：https://www.sylar.top/sblog/detail/42

## 01日志系统01

课程总结：服务器框架简介

**(1) 日志系统**

1. 服务器框架日志系统是一个很重要的东西，服务器出问题、做分析一般都会用到日志系统

2. Log4J 日志系统设计

```
Logger (定义日志类别)
   |
   |----------------Formatter (日志格式)
   |
Appender (日志输出的地方)
```

3. Logger - 输出日志的时候会获取一个Logger

**(2) 配置系统**

Config --> Yaml

配置日志要输出的类型，输出格式，输出的目的地，日志等级都可以设置

yamp - cpp: github

mkdir build && cd build && cmake .. && make install

```
下载地址
https://github.com/jbeder/yaml-cpp/releases/tag/yaml-cpp-0.6.3

/usr/local/lib64/libyaml-cpp.a
/usr/local/include/yaml-cpp
/usr/local/include/gmock
/usr/local/include/gtest
/usr/local/lib64/libgtest.a
```

**(3) 协程库封装**

1. 将一些异步的操作，封装成同步。
2. <u>协程与异步比较。</u>

**(4) socket函数库**

**(5) http协议开发**

**(6) 分布式协议**

1. 将逻辑业务抽象成一个一个插件，业务功能和系统功能拆分开

**(7) 推荐系统**



## 02日志系统02_logger



## 03日志系统03_appender



## 04日志系统04_formatter



## 05日志系统05_formatter2



## 06日志系统06编译调试



## 07日志系统07完善日志系统



## 08日志系统08完善日志系统2



## 09配置系统01基础配置



## 10配置系统02_yaml



## 11配置系统03_yaml整合



## 12配置系统04复杂类型的支持



## 13配置系统05更多stl容器支持



## 14配置系统06自定义类型的支持



## 15配置系统07配置变更事件





## 16配置系统08日志系统整合01



## 17配置系统09日志系统整合02



## 18配置系统10日志系统整合03



## 19配置系统11日志系统整合04



## 20配置系统12日志系统整合05







## 21线程模块01



## 22线程模块02信号量和互斥量



## 23线程模块03日志模块整合



## 24线程模块04日志模块整合02



## 25线程模块05配置模块整合



## 26协程模块01



## 27协程模块02



## 28协程模块03



## 29协程模块04



## 30协程调度模块01



## 31协程调度模块02



## 32协程调度模块03



## 33协程调度模块04



## 34协程调度模块05



## 35协程调度模块06



## 36_IO协程调度模块01



## 37_IO协程调度模块02



## 38_IO协程调度模块03



## 39_IO协程调度模块04



## 40_IO协程调度模块05定时器01



## 41_IO协程调度模块06定时器02



## 42_IO协程调度模块07定时器03



## 43_Socket_IO_Hook01



## 44_Socket_IO_Hook02



## 45_Socket_IO_Hook03



## 46_Socket_IO_Hook04



## 47_Socket_IO_Hook05



## 48_Socket_IO_Hook06



## 49网络模块Socket01



## 50网络模块Socket02



## 51网络模块Socket03



## 52网络模块Socket04



## 53网络模块Socket05



## 54网络模块Socket06



## 55网络模块Socket07



## 56网络模块Socket08



## 57序列化ByteArray01



## 58序列化ByteArray02
## 59序列化ByteArray03
## 60序列化ByteArray04
## 61_HTTP协议封装01
## 62_HTTP协议封装02
## 63_HTTP协议封装03
## 64_HTTP协议封装04
## 65_HTTP协议封装05
## 66_HTTP协议封装06
## 67_TcpServer封装01
## 68_TcpServer封装02
## 69_Stream封装SocketStream
## 70_HttpSession封装
## 71_HttpServer封装
## 72_HttpServlet封装01
## 73_HttpServlet封装02
## 74_HttpConnection封装01
## 75_HttpConnection封装02
## 76_HttpConnection封装03
## 77_HttpConnection封装04
## 78_HttpConnection封装05
## 79_HttpConnection封装06
## 80_基础模块篇总结*</u>