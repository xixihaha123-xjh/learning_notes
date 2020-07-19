# 1 mysql初始化和清理

1. mysql初始化函数

```
mysql_library_init()
mysql_library_end()
mysql_init()
mysql_close()
mysql_real_connect()
```

2. 与MySQL交互时， 应用程序应使用该一般性原则

```
1. 通过调用mysql_library_init()，初始化MySQL库。库可以是mysqlclient C客户端库，或mysqld嵌入式服务器库，具体情况取决于应用程序是否与“-libmysqlclient”或“-libmysqld”标志链接。
2. 通过调用mysql_init()初始化连接处理程序，并通过调用mysql_real_connect()连接到服务器。
3. 发出SQL语句并处理其结果。（在下面的讨论中，详细介绍了使用它的方法）。
4. 通过调用mysql_close()，关闭与MySQL服务器的连接。
5. 通过调用mysql_library_end()，结束MySQL库的使用。
```

# 2 mysql API开发

## 2.1 数据库连接登陆

### 2.1.1. mysql_real_connect

[参考官方网址 mysql_real_connect](https://dev.mysql.com/doc/refman/8.0/en/mysql-real-connect.html)

```c

MYSQL *mysql_real_connect(MYSQL *mysql, 
                          const char *host, 
                          const char *user, 
                          const char *passwd, 
                          const char *db, 
                          unsigned int port, 
                          const char *unix_socket, 
                          unsigned long client_flag)

1.对于第一个参数，指定一个现有MYSQL结构的地址。在调用mysql_real_connect()之前，调用mysql_init()来初始化MYSQL结构。可以使用mysql_options()调用更改很多连接选项。参见第28.7.6.50节，“mysql_options()”
主机的值可以是主机名或IP地址。客户端尝试连接如下:

2.主机的值可以是主机名或IP地址。如果主机为空或字符串“localhost”,即为本机

3.user参数包含用户的MySQL登录ID。如果user为NULL或空字符串“”，则假定当前用户。在Unix下，这是当前的登录名。在Windows ODBC下，必须显式地指定当前用户名
    
4.passwd参数包含用户密码
    
5.db是数据库名。如果db不为空，连接将默认数据库设置为这个值
    
6.如果端口不是0，该值将用作TCP/IP连接的端口号。注意，host参数决定连接的类型。默认3306
    
7.如果unix_socket不为空，则该字符串指定要使用的套接字或命名管道。注意，host参数决定连接的类型。
    
8.client_flag的值通常为0，但可以设置为以下几个标志的组合，以启用某些特性。
CLIENT_OPTIONAL_RESULTSET_METADATA:这个标志使结果集元数据成为可选的。禁止元数据传输可以提高性能，特别是对于执行许多查询但每个查询只返回很少行的会话.禁止传输表数据
```

2. 测试程序

```c++
int main()
{
	MYSQL mysql;
	mysql_init(&mysql);                      // 初始化mysql上下文
	const char* host = "127.0.0.1";
	const char* user = "root";
	const char* passwd = "123";
	const char* db = "mysql";                // 数据库名称
	if (!mysql_real_connect(&mysql, host, user, passwd, db, 3306, 0, 0))
	{
		std::cout << "mysql_connect failed!" << mysql_error(&mysql) << std::endl;
	}
	else
	{
		std::cout << "mysql_connect successed!" << std::endl;
	}
}
```

### 2.1.2 mysql_options

1. mysql_options

```c
int mysql_options(MYSQL *mysql, enum mysql_option option, const void *arg)

可以用来设置额外的连接选项并影响连接的行为。可以多次调用此函数来设置多个选项。要检索选项值，请使用mysql_get_option()。在mysql_init()之后和mysql_connect()之前调用mysql_options()或mysql_real_connect()
    
option参数是你要设置的选项;参数arg是该选项的值

下面的列表描述了可能的选项、它们的效果以及如何对每个选项使用arg
MYSQL_OPT_CONNECT_TIMEOUT (argument type: unsigned int *)
The connect timeout in seconds. 

MYSQL_OPT_READ_TIMEOUT (argument type: unsigned int *) 
每次尝试从服务器读取数据时的超时时间(以秒为单位)。如果需要，会有重试，所以总的有效超时值是选项值的三倍。可以设置该值，以便在TCP/IP Close_Wait_Timeout值为10分钟之前检测到连接丢失。
```

2. mysql_options 超时设定

```c++
// 设定超时时间
int time_out_period = 3;
mysql_options(&mysql, MYSQL_OPT_CONNECT_TIMEOUT, &time_out_period);
```

3. mysql_options 断开重连

```c++
// 自动重连
int reconnect = 1;
mysql_options(&mysql, MYSQL_OPT_RECONNECT, &reconnect);
结论：设置自动重连，停止mysql服务后，mysql重新启动后，自动连接
```

```c++
for (int i = 0; i < 1000; i++)
{
	int result = mysql_ping(&mysql);                // mysql_ping
	if (result == 0)
	{
		std::cout << host << ": mysql ping success" << std::endl;
	}
	else
	{
		std::cout << host << ": mysql ping failed" << mysql_error(&mysql) << std::endl;
	}
	std::this_thread::sleep_for(std::chrono::seconds(1));
}
结论：无自动重连，停止mysql服务后，mysql重新启动后，也无法连接
```

## 2.2 数据查询

### 2.2.1 执行sql语句

```c++
// user表： select * from user;
// 1. 执行sql语句
const char* sql = "select * from user";
// mysql_query       sql语句中只能是字符串
// int result = mysql_query(&mysql, sql);

// mysql_real_query  sql语句中可以包含二进制数据, 
// Commands out of sync; you can't run this command now，执行sql语句后，必须获取结果集，并且清理
int result = mysql_real_query(&mysql, sql, strlen(sql));
if (result != 0)
{
	cout << "mysql_real_query failed!" << sql << "  " << mysql_error(&mysql) << endl;
}
```



### 2.2.2 获取结果集

1. mysql_use_result

```c++
// 2. 获取结果集
MYSQL_RES* result_set =  mysql_use_result(&mysql);
if (!result_set)
{
	std::cout << "mysql_use_result failed" << mysql_error(&mysql) << std::endl;
}
```

* result_set 返回的结果集：row_count = 0 不实际读取数据

![mysql_use_result返回值](pic\1005 mysql_use_result返回值.png)

2. mysql_store_result：读取所有数据，注意缓存大小，缓存大小默认64M，可以通过mysql配置文件配置项设置或者通过mysql_options 函数 MYSQL_OPT_MAX_ALLOWED_PACKET设置

```c++
MYSQL_RES* result_set =  mysql_store_result(&mysql);
```

![mysql_store_result](pic\1006 mysql_store_result返回值.png)



### 2.2.3 清理资源

```c++
//3. 遍历结果集
MYSQL_ROW row;
while (row = mysql_fetch_row(result_set))
{
	unsigned long* length = mysql_fetch_lengths(result_set);
	std::cout << "[" << row[0] << "," << row[1] << "]" << std::endl;
}
// 4. 清理结果集
mysql_free_result(result_set);
```

## 2.3 表结构获取

```c++
// 获取表字段
MYSQL_FIELD *field = 0;
while (field = mysql_fetch_field(result_set))
{
	std::cout << "key:" << field->name << std::endl;
}
// 获取表字段数量
int field_num = mysql_num_fields(result_set);
```

## 2.4 表创建、数据插入和修改

### 2.4.1 创建表

```c++
// 1. 创建表
std::string sql = "create table if not exists t_image ( \
	id int auto_increment, \
	name varchar(1024), \
	path varchar(1024), \
	size int, \
	primary key(id))";
int result = mysql_real_query(&mysql, sql.c_str(), strlen(sql.c_str()));
```

### 2.4.2 插入数据

```c++
// 2. 插入数据
sql = "insert t_image(name, path, size) values('test.jpg', '/root/home/', 10240)";
result = mysql_real_query(&mysql, sql.c_str(), strlen(sql.c_str()));
if (result != 0)
{
	cout << "mysql_real_query failed!" << sql << endl;
}
int count = mysql_affected_rows(&mysql);
cout << "mysql_affected_rows:  " << count << "id = " << mysql_insert_id(&mysql) << endl;
```

### 2.4.3 更新数据

```c++
sql = "update t_image set name = 'Nihaowa.jpg', size = 1028 where id = 1";
result = mysql_real_query(&mysql, sql.c_str(), strlen(sql.c_str()));
if (result != 0)
{
	cout << "mysql_real_query failed!" << sql << "  " << mysql_error(&mysql) << endl;
}

// 根据map拼接sql语句
```

### 2.4.4 删除数据

```c++
delete             // 可回滚，只是标识，不会实际删除数据
truncate           // 清空数据
drop
```

## 2.5 一次执行多条sql语句

### 2.5.1 CLIENT_MULTI_STATEMENTS

```c++
// 支持多条sql语句 CLIENT_MULTI_STATEMENTS
if (!mysql_real_connect(&mysql, host, user, passwd, db, 3306, 0, CLIENT_MULTI_STATEMENTS))
{
	std::cout << "mysql_connect failed!" << mysql_error(&mysql) << std::endl;
}
else
{
	std::cout << "mysql_connect successed!" << std::endl;
}

// 1. 创建表
std::string sql = "create table if not exists t_image ( \
	id int auto_increment, \
	name varchar(1024), \
	path varchar(1024), \
	size int, \
	primary key(id));";

// 清空数据
sql += "truncate t_image;";
// 增
sql += "insert t_image(name, path, size) values('test.jpg', '/root/home/', 1234);";
// 改
sql += "update t_image set name = 'Nihaowa.jpg', size = 4321 where id = 1;";
// 查
sql += "select * from t_image;";
// 执行sql语句立刻返回，但语句并没有全部执行，需要获取结果
// 把sql整个发送给mysql server, server一条条执行，返回结果
int result = mysql_query(&mysql, sql.c_str());
if (result != 0)
{
	cout << "mysql_query failed!" << sql << "  " << mysql_error(&mysql) << endl;
}
```

### 2.5.2 mysql_next_result

