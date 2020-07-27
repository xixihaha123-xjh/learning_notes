

## 需注意

### 1 公网554端口

公网现在一般都封了samba监听端口554，如果需要正常访问，需要：

1.改变samba服务器的监听端口；

2.在windows下做端口映射，将对554的访问，改为修改后的samba端口连接；

### 2 samba用户

 添加的Samba用户首先必须是Linux用户

## samba安装

1. 安装

```
yum -y install samba
```

2. 测试安装是否成功

```
systemctl start smb.service
systemctl start nmb.service
```

3. 看运行状态

```
systemctl status nmb.service
```

4. 设置开机启动

```
chkconfig smb on
chkconfig nmb on
```



## windows客户端

samba服务器已经部署完了，现在需要下载一个第三方工具，进行端口映射。

参考：腾讯云安装samba服务器无法连接问题

```
divertTCPconn.exe 445 8445
```

