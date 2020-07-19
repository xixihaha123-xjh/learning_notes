# zookeeper概述

为分布式应用提供协调服务：Zookeeper 动物管理员，管理这整个大数据中n多个框架（hadoop，kafka，hbase）

Apache项目：hadoop  spring

------

**Zookeeper 工作机制：**

Zookeeper从设计模式角度来理解：

是一个基于观察者模式设计的分布式服务管理框架，它负责存储和管理大家都关心的数据，然后接受观察者的注册，一旦这些数据的状态发生变化，Zookeeper就将负责通知已经在Zookeeper上注册的那些观察者做出相应的反应，从而实现集群中类似Master/Slave管理模式



Zookeeper=文件系统+通知机制

------

**数据结构：**

树形文件系统：linux、HDFS



------

**应用场景：**



------

