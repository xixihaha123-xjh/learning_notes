# Docker基础

# 1 简介

## 1.1 快速上手

（1）前提知识

1. 熟练掌握Linux命令和相关背景知识 ps - top

2. Maven/Git相关的知识 git pull

（2）课程定位

```c
JavaEE    java       Springmvc/Springboot/mybatis
Docker    Go         Swarm/Compose/Machine/mesos/K8s/ ----- CI/CD
```

## 1.2 是什么?

（1）问题：为什么会有docker出现？

一款产品从开发到上线，从操作系统，到运行环境，再到应用配置。作为开发+运维之间的协作需要关心很多西，这也是很多互联网公司都不得不面对的问题，特别是各种版本的迭代之后，不同版本环境的兼容，对运维人员都是考验。

Docker之所以发展如此迅速，也是因为它对此给出了一个标准化的解决方案。

环境配置如此麻烦，换一台机器，就要重来一次，费力费时。很多人想到，能不能从根本上解决问题，软件可以带环境安装？也就是说，安装的时候，把原始环境一模一样地复制过来。开发人员利用 Docker 可以消除协作编码时“在我的机器上可正常工作”的问题。 

![docker理念](pic\11-docker理念.png)

之前在服务器配置一个应用的运行环境，要安装各种软件，就拿尚硅谷电商项目的环境来说吧，Java /Tomcat /MySQL /JDBC  驱动包等。安装和配置这些东西有多麻烦就不说了，它还不能跨平台。假如我们是在 Windows 上安装的这些环境，到了 Linux 又得重新装。况且就算不跨操作系统，换另一台同样操作系统的服务器，要移植应用也是非常麻烦的。

传统上认为，软件编码开发/测试结束后，所产出的成果即是程序或是能够编译执行的二进制字节码等(java为例)。而为了让这些程序可以顺利执行，开发团队也得准备完整的部署文件，让维运团队得以部署应用，开发需要清楚的告诉运维部署团队，用的全部配置文件+所有软件环境。不过，即便如此，仍然常常发生部署失败的状况。Docker镜像的设计，使得Docker得以打破过去「程序即应用」的观念。**透过镜像(images)将作业系统核心除外，运作应用程式所需要的系统环境，由下而上打包，达到应用程式跨平台间的无缝接轨运作。**

（2）docker理念

Docker是基于Go语言实现的云开源项目。Docker的主要目标是“Build，Ship and Run Any App, Anywhere”。 也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的APP（可以是一个WEB应用或数据库应用等等）及其运行环境能够做到“一次封装，到处运行”。 

Linux 容器技术的出现就解决了这样一个问题，而 Docker 就是在它的基础上发展过来的。将应用运行在 Docker 容器上面，而 Docker 容器在任何操作系统上都是一致的，这就实现了跨平台、跨服务器。只需要一次配置好环境，换到别的机子上就可以一键部署好，大大简化了操作



（3）总结

解决了运行环境和配置问题的软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术。

# 2 docker 作用

## 2.1 之前的虚拟化技术

（1）虚拟机（virtual machine）就是带环境安装的一种解决方案。

1. 它可以在一种操作系统里面运行另一种操作系统，比如在Windows 系统里面运行Linux 系统。应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通文件，不需要了就删掉，对其他部分毫无影响。这类虚拟机完美的运行了另一套系统，能够使应用程序，操作系统和硬件三者之间的逻辑不变。 

<img src="pic\12-虚拟机.png" alt="虚拟机" style="zoom:80%;" />

（2）虚拟机的缺点： 1 资源占用多   2 冗余步骤多   3 启动慢

## 2.2 容器虚拟化技术

（1）由于前面虚拟机存在这些缺点，Linux 发展出了另一种虚拟化技术：Linux 容器（Containers，LXC）。

1. Linux 容器不是模拟一个完整的操作系统，而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。

<img src="pic\13-docker.png" alt="docker" style="zoom:75%;" />

（2）比较了 Docker 和传统虚拟化方式的不同之处：

1. 传统虚拟机技术是虚拟出一套硬件后，在其上运行一个完整操作系统，在该系统上再运行所需应用进程；

2. 而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便。

3. 每个容器之间互相隔离，每个容器有自己的文件系统 ，容器之间进程不会相互影响，能区分计算资源。

## 2.3 开发/运维

（1）一次构建、随处运行

1. 更快速的应用交付和部署： 传统的应用开发完成后，需要提供一堆安装程序和配置说明文档，安装部署后需根据配置文档进行繁杂的配置才能正常运行。Docker化之后只需要交付少量容器镜像文件，在正式生产环境加载镜像并运行即可，应用安装配置在镜像里已经内置好，大大节省部署配置和测试验证时间。 
2. 更便捷的升级和扩容缩容：随着微服务架构和Docker的发展，大量的应用会通过微服务方式架构，应用的开发构建将变成搭乐高积木一样，每个Docker容器将变成一块“积木”，应用的升级将变得非常容易。当现有的容器不足以支撑业务处理时，可通过镜像运行新的容器进行快速扩容，使应用系统的扩容从原先的天级变成分钟级甚至秒级。 
3. 更简单的系统运维： 应用容器化运行后，生产环境运行的应用可与开发、测试环境的应用高度一致，容器会将应用程序相关的环境和状态完全封装起来，不会因为底层基础架构和操作系统的不一致性给应用带来影响，产生新的BUG。当出现程序异常时，也可以通过测试环境的相同容器进行快速定位和修复。 
4. 更高效的计算资源利用：Docker是内核级虚拟化，其不像传统的虚拟化技术一样需要额外的Hypervisor支持，所以在一台物理机上可以运行很多个容器实例，可大大提升物理服务器的CPU和内存的利用率。 

## 2.4 企业级

（1）新浪

（2）美团



（3）蘑菇街

#  3 常用命令

## 3.1 下载安装



## 3.2 镜像命令



```
# docker中的centos仅有215MB
➜  ~  docker images
REPOSITORY       TAG           IMAGE ID         CREATED         SIZE
centos           latest        831691599b88     12 days ago     215MB
hello-world      latest        bf756fb1ae65     5 months ago    13.3kB
```

## 3.3 容器命令

（1）新建并启动容器

```shell
➜  ~ docker run -it 831691599b88  # 新建并启动容器
[root@fd41183a3521 /]#
```

```
docker run [options] IMAGE [COMMAND] [ARG...]
```

```
OPTIONS说明（常用）：有些是一个减号，有些是两个减号
--name="容器新名字": 为容器指定一个名称；
-d: 后台运行容器，并返回容器ID，也即启动守护式容器；
-i：以交互模式运行容器，通常与 -t 同时使用；
-t：为容器重新分配一个伪输入终端，通常与 -i 同时使用；
-P: 随机端口映射；
-p: 指定端口映射，有以下四种格式
      ip:hostPort:containerPort
      ip::containerPort
	  hostPort:containerPort
      containerPort
```

（2）列出当前所有正在运行的容器

```
➜  ~ docker ps   # 查看正在运行的docker容器,鲸鱼背上的集装箱
CONTAINER ID   IMAGE         COMMAND      CREATED       STATUS    PORTS    NAMES
70058b4a6b23  831691599b88  "/bin/bash"  5 seconds ago  Up               determined_ellis
```

```
docker ps [options]
OPTIONS说明（常用）：
-a :列出当前所有正在运行的容器+历史上运行过的
-l :显示最近创建的容器。
-n：显示最近n个创建的容器。
-q :静默模式，只显示容器编号。
--no-trunc :不截断输出。
```

（3）退出容器

```
exit       容器停止退出
ctrl+P+Q   容器不停止退出
```

（4）启动容器

```
docker start 容器ID或者容器名
```

（5）重启容器

```
docker restart 容器ID或者容器名
```

（6）停止容器 - 温柔关闭

```
docker stop 容器ID或者容器名
```

（7）强制停止容器 -- 相当于直接拔电源

```
docker kill 容器ID或者容器名
```

（8）删除已停止的容器

```
docker rm 容器ID
docker rm -f $(docker ps -a -q)
docker ps -a -q | xargs docker rm
```

（9）启动守护式容器

```
➜  ~ docker run -d centos
759c74fb05b0abb7c006caeac7e1c2ec9082bf67eed2cbf51b9b828ba2c55814

# 使用镜像centos:latest以后台模式启动一个容器
docker run -d centos

问题：然后docker ps -a 进行查看, 会发现容器已经退出
很重要的要说明的一点: Docker容器后台运行,就必须有一个前台进程.
容器运行的命令如果不是那些一直挂起的命令（比如运行top，tail），就是会自动退出的。

这个是docker的机制问题,比如你的web容器,我们以nginx为例，正常情况下,我们配置启动服务只需要启动响应的service即可。例如

service nginx start
但是,这样做,nginx为后台进程模式运行,就导致docker前台没有运行的应用,
这样的容器后台启动后,会立即自杀因为他觉得他没事可做了.
所以，最佳的解决方案是,将你要运行的程序以前台进程的形式运行
```

（10） 查看容器日志

```shell
➜  ~ docker run -d centos /bin/sh -c "while true;do echo zzyy;sleep 2;done"
827c6bb5ea27cac3a45d548d5b26c42db69f0332b14a484a5d7765eb1949d90f

➜  ~ docker logs -f -t --tail 100 827c6bb5ea27
2020-06-29T13:18:36.912609359Z zzyy
2020-06-29T13:18:38.914291527Z zzyy

-t 是加入时间戳
-f 跟随最新的日志打印
--tail 数字 显示最后多少条

➜  ~ docker ps
CONTAINER ID    IMAGE   COMMAND                  CREATED      STATUS  PORTS       NAMES
827c6bb5ea27   centos   "/bin/sh -c 'while t…" 15 seconds ago  Up        pensive_brattain
```

（11）查看容器内运行的进程

```
docker top 容器ID
~ docker top 827c6bb5ea27
```

（12）查看容器内部细节

```
docker inspect 容器ID
➜  ~ docker inspect 827c6bb5ea27
```

（13）进入正在运行的容器并以命令行交互

```
➜  ~ docker run -it centos /bin/bash
[root@82c6b5463023 /]# #                                                                                                                                                     
➜  ~ docker attach 82c6b5463023
[root@82c6b5463023 /]#

➜  ~ docker exec -it 82c6b5463023 ls -l /tmp
exec 是在容器中打开新的终端,并且可以启动新的进程
```

（14）从容器内拷贝文件到主机上

```
docker cp 容器ID:容器内路径  目标主机路径
➜  ~ docker cp 82c6b5463023:/tmp/ks-script-z6zw_bhq /home/
➜  /home ls
ks-script-z6zw_bhq  user1  user2
```

（15）总结

<img src="pic\1-docker命令.png" alt="docker命令 " style="zoom:75%;" />

```
# 当前 shell 下 attach 连接指定运行镜像
attach    Attach to a running container
# 通过 Dockerfile 定制镜像
build     Build an image from a Dockerfile
# 提交当前容器为新的镜像
commit    Create a new image from a container changes   
#从容器中拷贝指定文件或者目录到宿主机中
cp        Copy files/folders from the containers filesystem to the host path   
 # 创建一个新的容器，同 run，但不启动容器
create    Create a new container                       
# 查看 docker 容器变化
diff      Inspect changes on a container's filesystem
# 从 docker 服务获取容器实时事件
events    Get real time events from the server
# 在已存在的容器上运行命令
exec      Run a command in an existing container
# 导出容器的内容流作为一个 tar 归档文件[对应 import ]
export    Stream the contents of a container as a tar archive
# 展示一个镜像形成历史
history   Show the history of an image 
# 列出系统当前镜像
images    List images
# 从tar包中的内容创建一个新的文件系统映像[对应export]
import    Create a new filesystem image from the contents of a tarball
# 显示系统相关信息
info      Display system-wide information
# 查看容器详细信息
inspect   Return low-level information on a container
# kill 指定 docker 容器
kill      Kill a running container
# 从一个 tar 包中加载一个镜像[对应 save]
load      Load an image from a tar archive              
# 注册或者登陆一个 docker 源服务器
login     Register or Login to the docker registry server
# 从当前 Docker registry 退出
logout    Log out from a Docker registry server
# 输出当前容器日志信息
logs      Fetch the logs of a container
# 查看映射端口对应的容器内部源端口
port      Lookup the public-facing port which is NAT-ed to PRIVATE_PORT
# 暂停容器
pause     Pause all processes within a container
# 列出容器列表
ps        List containers
# 从docker镜像源服务器拉取指定镜像或者库镜像
pull      Pull an image or a repository from the docker registry server
# 推送指定镜像或者库镜像至docker源服务器
push      Push an image or a repository to the docker registry server
# 重启运行的容器
restart   Restart a running container
# 移除一个或者多个容器
rm        Remove one or more containers
# 移除一个或多个镜像[无容器使用该镜像才可删除，否则需删除相关容器才可继续或 -f 强制删除]
rmi       Remove one or more images

# 创建一个新的容器并运行一个命令
run       Run a command in a new container
# 保存一个镜像为一个 tar 包[对应 load]
save      Save an image to a tar archive
# 在 docker hub 中搜索镜像
search    Search for an image on the Docker Hub
# 启动容器
start     Start a stopped containers
# 停止容器
stop      Stop a running containers                     
# 给源中镜像打标签
tag       Tag an image into a repository
# 查看容器中运行的进程信息
top       Lookup the running processes of a container
# 取消暂停容器
unpause   Unpause a paused container
# 查看 docker 版本号
version   Show the docker version information
# 截取容器停止时的退出状态值
wait      Block until a container stops, then print its exit code 
```

# 4 docker镜像

思考：tomcat镜像为什么会那么大? 462.6MB

镜像就是千层饼，就是一层套一层的花卷

## 4.1 是什么?

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。 

（1）UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，**它支持对文件系统的修改作为一次提交来一层层的叠加**，同时可以将不同目录挂载到同一个虚拟文件系统下(unite several directories into a single virtual filesystem)。Union 文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

![联合文件系统](pic\2-联合文件系统.png)

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录 

（2）Docker镜像加载原理：

docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS。

bootfs(boot file system)主要包含bootloader和kernel，bootloader主要是引导加载kernel, Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs (root file system) ，在bootfs之上。包含的就是典型 Linux 系统中的 /dev, /proc, /bin, /etc 等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等

![docker镜像加载](pic\3-docker镜像加载.png)

平时我们安装进虚拟机的CentOS都是好几个G，为什么docker这里才200M？？

对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接用Host的kernel，自己只需要提供 rootfs 就行了。由此可见对于不同的linux发行版, bootfs基本是一致的, rootfs会有差别, 因此不同的发行版可以公用bootfs。

（3） 分层镜像

以我们的pull为例，在下载的过程中我们可以看到docker的镜像好像是在一层一层的在下载 

![分层的镜像](pic\4-分层的镜像.png)

（4）为什么Docker镜像要采用这种分层结构呢？

最大的一个好处就是 - 共享资源

比如：有多个镜像都从相同的 base 镜像构建而来，那么宿主机只需在磁盘上保存一份base镜像，

同时内存中也只需加载一份 base 镜像，就可以为所有容器服务了。而且镜像的每一层都可以被共享。



tomcat image = kernel + centos +jdk8 + tomcat

<img src="pic\5-tomcat分层文件系统.png" alt="tomcat分层文件系统" style="zoom:50%;" />

## 4.2 镜像特点

1. Docker镜像都是只读的
2. 当容器启动时，一个新的可写层被加载到镜像的顶部，这一层通常被称作容器层，容器层之下的都叫做镜像层

## 4.3 镜像commit操作补充

（1）docker commit提交容器副本使之成为一个新的镜像

```
docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]
```

（2）案例演示

1. 从Hub上下载tomcat镜像到本地并成功运行 

<img src="pic\7-commit案例.png" alt="commit案例" style="zoom:75%;" />

```
1.从Hub上下载tomcat镜像到本地并成功运行 
docker run -it -p 8888:8080 tomcat
docker run -it -P tomcat
-p:主机端口:docker 容器端口
-P:随机分配端口
i: 交互
t: 终端
```

2. 故意删除上一步镜像生产tomcat容器的文档

![commit案例](pic\6-commit案例.png)



3. 也即当前的tomcat运行实例是一个没有文档内容的容器,以它为模板commit一个没有doc的tomcat新镜像guigu/tomcat

```
docker commit -a="xixihaha123" -m="delete tomcat docs" xxxxxxxxx guigu/tomcat:1.2
```

![commit案例](pic\8-commit案例.png)



4. 启动我们的新镜像并和原来的对比，启动guigu/tomcat，它没有docs，新启动原来的tomcat，它有docs

![commit案例](pic\9-commit案例.png)



![commit案例](pic\10-commit案例.png)



# 5 docker容器

## 5.1 docker容器



## 5.2 容器数据卷

### 5.2.1 是什么?

（1）先来看看Docker的理念：

* 将应用与运行的环境打包形成容器运行 ，运行可以伴随着容器，但是我们对数据的要求希望是持久化的

* 容器之间希望有可能共享数据

（2）Docker容器产生的数据，如果不通过docker commit生成新的镜像，使得数据做为镜像的一部分保存下来，那么当容器删除后，数据自然也就没有了。

（3）为了能保存数据在docker中我们使用卷。

### 5.2.2 能干嘛?

（1） 作用：

1. 容器的持久化
2. 容器间继承 +共享数据 （容器到主机  或 主机到容器 或 容器与容器之间数据共享）

（2）卷就是目录或文件，存在于一个或多个容器中，由docker挂载到容器，但不属于联合文件系统，因此能够绕过Union File System提供一些用于持续存储或共享数据的特性：卷的设计目的就是数据的持久化，完全独立于容器的生存周期，因此Docker不会在容器删除时删除其挂载的数据卷

（3）特点：

1. 数据卷可在容器之间共享或重用数据

2. 卷中的更改可以直接生效
3. 数据卷中的更改不会包含在镜像的更新中

4. 数据卷的生命周期一直持续到没有容器使用它为止

### 5.2.3 数据卷

（1）直接命令添加

```
 docker run -it -v /宿主机目录:/容器内目录 centos /bin/bash
 
 docker run -it -v /dataVolume:/dataVolumeContainer centos
```

```
"Mounts": [
            {
                "Type": "bind",
                "Source": "/dataVolume",
                "Destination": "/dataVolumeContainer",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ]
```

1. 查看数据卷是否挂载成功
2. 容器和宿主机之间数据共享
3. 容器停止退出后，主机修改后数据是否同步
4. 命令（带权限）----  主机文件和容器数据可以同步，但容器内只允许读操作

```shell
docker run -it -v /hostdata:/dockerContainer:ro centos
```

```json
"Mounts": [
            {
                "Type": "bind",
                "Source": "/hostdata",
                "Destination": "/dockerContainer",
                "Mode": "ro",
                "RW": false,
                "Propagation": "rprivate"
            }
        ]
```



（2）DockerFile添加

1. 根目录下新建mydocker文件夹并进入

2. 可在dockerfile中使用 `VOLUME指令 ` 来给镜像添加一个或多个数据卷

3. File构建

```
# volume test
FROM centos
VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
CMD echo "finished,--------success1"
CMD /bin/bash
```

4. build 后生成镜像

```shell
➜  /mydocker docker build -f /mydocker/dockerfile -t xixihaha123/centos .
Sending build context to Docker daemon  2.048kB
Step 1/4 : FROM centos
 ---> 831691599b88
Step 2/4 : VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
 ---> Running in d7ebc804bf34
Removing intermediate container d7ebc804bf34
 ---> cf10e81b8141
Step 3/4 : CMD echo "finished,--------success1"
 ---> Running in 2df327629e5a
Removing intermediate container 2df327629e5a
 ---> 902a7cfadbd4
Step 4/4 : CMD /bin/bash
 ---> Running in 34389a9888a5
Removing intermediate container 34389a9888a5
 ---> da3459170bc8
Successfully built da3459170bc8
Successfully tagged xixihaha123/centos:latest
➜  /mydocker docker images
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
xixihaha123/centos   latest              da3459170bc8        13 seconds ago      215MB
```



```
➜  /mydocker docker run -it xixihaha123/centos /bin/bash
[root@1b0d29901e63 /]# ls
dataVolumeContainer2 dataVolumeContainer1 
```

```
Mounts": [
            {
                "Type": "volume",
                "Name": "e0485b851b09417eee74b4fe88288013c56943be2bad1ca7522451e2d8c2ee0e",
                "Source": "/var/lib/docker/volumes/e0485b851b09417eee74b4fe88288013c56943be2bad1ca7522451e2d8c2ee0e/_data",           # 主机默认路径
                "Destination": "/dataVolumeContainer1",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ]

```

### 5.2.4 数据卷容器

（1）命名的容器挂载数据卷，其它容器通过挂载这个(父容器)实现数据共享，挂载数据卷的容器，称之为数据卷容器

（2）总体介绍



（3）容器间传递共享（volumes-from）



# 6 dockerfile解析

1. 手动编写一个dockerfile 文件，当然，必须要符合file的规范

2. 有这个文件后，直接docker build 命令执行，获得一个自定义的镜像
3. run

## 6.1 是什么?

（1）dockerfile是用来构建docker镜像的构架文件，是由一系列命令和参数构成的脚步

（2）构建三步骤

1. 编写dockerfile文件
2. docker build
3. docker run

（3）文件怎么样？

以我们熟悉的centos6.9为例

```shell
FROM scratch
ADD centos-6-docker.tar.xz /

LABEL name="CentOS Base Image" \
    vendor="CentOS" \
    license="GPLv2" \
    build-date="20170406"

CMD ["/bin/bash"]
```

## 6.2 dockerfile构建过程

（1） Dockerfile内容基础知识

1. 每条保留字指令都必须为大写字母且后面要跟随至少一个参数
2. 指令按照从上到下，顺序执行

3. `  # ` 表示注释

4. 每条指令都会创建一个新的镜像层，并对镜像进行提交

（2）Docker执行Dockerfile的大致流程

1. docker从基础镜像运行一个容器
2. 执行一条指令并对容器做出修改
3. 执行类似docker commit的操作提交一个新的镜像层
4. docker再基于刚提交的镜像运行一个新容器
5. 执行dockerfile中的下一条指令直到所有指令都执行完成

（3）总结

1. 从应用软件的角度来看，Dockerfile、Docker镜像与Docker容器分别代表软件的三个不同阶段，

* Dockerfile是软件的原材料

* Docker镜像是软件的交付品

* Docker容器则可以认为是软件的运行态。

Dockerfile面向开发，Docker镜像成为交付标准，Docker容器则涉及部署与运维，三者缺一不可，合力充当Docker体系的基石。

![dockerfile与镜像与容器](pic\51-dockerfile与镜像与容器.png)

1 Dockerfile，需要定义一个Dockerfile，Dockerfile定义了进程需要的一切东西。Dockerfile涉及的内容包括执行代码或者是文件、环境变量、依赖包、运行时环境、动态链接库、操作系统的发行版、服务进程和内核进程(当应用进程需要和系统服务和内核进程打交道，这时需要考虑如何设计namespace的权限控制)等等;

2 Docker镜像，在用Dockerfile定义一个文件之后，docker build时会产生一个Docker镜像，当运行 Docker镜像时，会真正开始提供服务;

3 Docker容器，容器是直接提供服务的。

## 6.3 dockerfile保留字指令

<img src="pic\54-dockerfile指令.png" alt="dockerfile指令 " style="zoom:65%;" />

（1）FROM：基础镜像，当前新镜像是基于哪个镜像的

（2）MAINTAINER：镜像维护者的姓名和邮箱地址

（3） RUN：容器构建时需要运行的命令

（4） EXPOSE：当期容器对外暴露的端口

（5）WORKDIR：指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点

（6） ENV：用来构建镜像过程中设置环境变量

```
ENV MY_PATH /usr/mytest
这个环境变量可以在后续的任何RUN指令中使用，这就如同在命令前面指定了环境变量前缀一样；
也可以在其它指令中直接使用这些环境变量，
比如：WORKDIR $MY_PATH
```

（7）ADD：将宿主机目录下的文件拷贝进镜像且ADD命令会自动处理url和解压tar压缩包

（8）COPY：类似ADD，拷贝文件和目录到镜像中。将构建上下文目录中<源路径>的文件/目录复制到新的一层的镜像内的<目标路径>位置

```dockerfile
COPY src dest
COPY ["src", "dest"]
```

（9）VOLUME：容器数据卷，用于数据保存和持久化工作

（10）CMD：指定一个容器启动时要运行的命令

dockerfile中可以有多个CMD指令，但只有最后一个生效，CMD会被docker run之后的参数替换

![cmd命令](pic\52-cmd命令容器.png)

（11）ENTRYPOINT：指定一个容器启动时要运行的命令，ENTRYPOINT的目的和CMD一样，都是在指定容器启动程序及参数

（12）ONBUILD：当构建一个被继承的Dockerfile时运行命令，父镜像在被子继承后父镜像的onbuild被触发

![onbuild指令](pic\53-onbuild指令.png)



## 6.4 案例

（1）Base镜像（scratch）：docker hub中99%的镜像都是通过在base镜像中安装和配置需要的软件构建出来的

![](F:\00-C++后台服务器学习\02-操作系统\04-docker\pic\55-dockerfile scratch.png)

（2）自定义镜像 mycentos

1. 编写：

* Hub默认Centos镜像是什么情况

![centos镜像](pic\56-centos镜像.png)

```
自定义mycentos目的使我们自己的镜像具备如下：
登陆后的默认路径
vim编辑器
查看网络配置ifconfig支持
```

* 准备编写dockerfile文件

![编写dockerfile文件](pic\57-编写dockerfile文件.png)

* myCentos内容DockerFile

```dockerfile
FROM centos
MAINTAINER Nihaowa<Nihaowa@163.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "success--------------ok"
CMD /bin/bash
```

2. 构建：

```shell
docker build -t 新镜像名字:TAG .            # 会看到 docker build 命令最后有一个 . 表示当前目录
```

![dockerfile构建](pic\58-dockerfile构建.png)

3. 运行

```
docker run -it 新镜像名字:TAG
```

*  可以看到，我们自己的新镜像已经支持vim/ifconfig命令，扩展成功了。 

![docker run](pic\59-docker run.png)

4. 列出镜像的变更历史

```shell
docker history 镜像名
```

![history](pic\50-dockerfile案例1.png)

（3）CMD/ENTRYPOINT镜像案例

1. 都是指定一个容器启动时要运行的命令
2. CMD：dockerfile中可以有多个CMD指令，但只有最后一个生效，CMD会被docker run之后的参数替换

```
docker run -it -p 8888:8080 tomcat ls -l
```



2. ENTRYPOINT：

（4）自定义镜像Tomcat 9

```
curl http://www.baidu.com
```





# 通俗解释Docker是什么？

Docker的思想来自于集装箱，集装箱解决了什么问题？在一艘大船上，可以把货物规整的摆放起来。并且各种各样的货物被集装箱标准化了，集装箱和集装箱之间不会互相影响。那么我就不需要专门运送水果的船和专门运送化学品的船了。只要这些货物在集装箱里封装的好好的，那我就可以用一艘大船把他们都运走。

docker就是类似的理念。现在都流行云计算了，云计算就好比大货轮。docker就是集装箱。

1.不同的应用程序可能会有不同的应用环境，比如.net开发的网站和php开发的网站依赖的软件就不一样，如果把他们依赖的软件都安装在一个服务器上就要调试很久，而且很麻烦，还会造成一些冲突。比如IIS和Apache访问端口冲突。这个时候你就要隔离.net开发的网站和php开发的网站。常规来讲，我们可以在服务器上创建不同的虚拟机在不同的虚拟机上放置不同的应用，但是虚拟机开销比较高。docker可以实现虚拟机隔离应用环境的功能，并且开销比虚拟机小，小就意味着省钱了。

2.你开发软件的时候用的是Ubuntu，但是运维管理的都是centos，运维在把你的软件从开发环境转移到生产环境的时候就会遇到一些Ubuntu转centos的问题，比如：有个特殊版本的数据库，只有Ubuntu支持，centos不支持，在转移的过程当中运维就得想办法解决这样的问题。这时候要是有docker你就可以把开发环境直接封装转移给运维，运维直接部署你给他的docker就可以了。而且部署速度快。

3.在服务器负载方面，如果你单独开一个虚拟机，那么虚拟机会占用空闲内存的，docker部署的话，这些内存就会利用起来。

总之docker就是集装箱原理。

-------------------------------------------------------------------------------------------------------------------------------------------------



 https://mp.weixin.qq.com/s/6dmGa6IwWVZfSCWmnnuq3g 

