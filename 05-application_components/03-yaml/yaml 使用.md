

# Yaml学习

# 1 定义

（1）Yaml 可以理解成一种标记语言也可以理解成非标记语言。

Yet Another Markup Language 是一个用来表达数据序列化的格式，具有高可读性（文本格式规范）。可以理解成一种标记语言。

Yaml 以数据作为中心，而不是以标记语言为重点（标记语言包含文本及文本相关的格式信息），yaml理解为是一种非标记语言。

 YAML 是 "YAML Ain't a Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。 

# 2 Yaml与xml和json比较

Yaml格式文件：

```yaml
cities: 
 city: 
  - name: Beijing
    id: 1
```

xml格式文件：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<cities>
  <city>
    <name>Beijing</name>
    <id>1</id>
  </city>
</cities>
```

json格式文件：

```json
{
    "cities":[
        {"name": "Beijing", "id": 1}
    ]
}
```

1-Yaml比xml或json更清晰。对于xml和json的表达，对于每一次节点，你都要去找结尾标记，并和节点开头标记对于起来看，但是 Yaml 则完全不用，它利用了人阅读的时候，一行一行往下顺序进行的特性，利用直观的缩进块来表达一个特定深度的节点。

2 对于某些强调层次结构的特定信息表达的场景，比如说电子邮件消息，或者是商品信息、候选人的简历等等，使用  Yaml  比其它的数据交换格式都要直接和清晰。

3 类比Python，"有意义"的缩进，Yaml也具备这样的特点

4 Yaml由于极强的可读性，它在数据类型的明确性上做了一定程度的牺牲。本来我们希望 name 是字符串，id 是数值，可是 Yaml 它根本不关心这一点，如你所见，字符串也没有双引号修饰，它只是诚实地把具体的文本数值罗列展示出来罢了。这一点，在我们权衡不同的数据交换格式的时候（比如设计哪一种格式作为我们的模块配置项文件），需要注意。

# 3 作用

1 配置文件

2 文件大纲

# 4 Yaml 语法规则

参考文件： https://zhuanlan.zhihu.com/p/96831410 

YAML在线格式化校验工具 http://www.bejson.com/validators/yaml/ 

YAML入门教程： https://www.runoob.com/w3cnote/yaml-intro.html 

[yaml介绍]: https://www.sylar.top/sblog/detail/34	"yaml介绍"
[yaml官方文档]: https://github.com/jbeder/yaml-cpp/wiki

极客时间 - 全栈工程师修炼指南 - 39 | xml、JSON、Yaml比较



## 4.1 语法规则

1. 大小写敏感

2. 使用缩进表示层级关系

3. 缩进时不允许使用Tab键，只允许使用空格，这一点与python不同

4. 缩进的空格数目不重要，只要保证不同层次的缩进空格数量不同即可

5. ` # `表示注释，从这个字符一直到行尾，都会被解析器忽略

```yaml
logs:
    - name: root
      level: info
      appenders:
          - type: FileLogAppender
            file: root.txt
          - type: StdoutLogAppender
```

```json
{ "logs": 
	[ { "name": "root",
		"level": "info",
      	"appenders": 
      	[ { "type": "FileLogAppender", "file": "root.txt" },
          { "type": "StdoutLogAppender" }
      	]
      }
    ]
}
```



## 4.2 数据类型

（1）Yaml支持以下几种数据类型：

1. 对象：键值对的集合，又称为映射(mapping)、哈希(hashes)、字典(dictionary)
2. 数组：一组按次序排列的值，又称为序列(sequence)、列表(list)
3. 纯量(scalars)：单个的、不可再分的值

### 4.2.1 纯量

（1）字符串

```yaml
# 双引号或者单引号包裹特殊字符，字符串可以拆成多行，每一行会被转成一个空格
string:
  - "Hello World" # 可以使用双引号或者单引号包裹特殊字符
  - newline_1
    newline_2
```

```
{ string: [ 'Hello World', 'newline_1 newline_2' ] }
```

2. 布尔值

```yaml
boolean:
  - true          # true ,True都可以
  - false         # false, False都可以
```

```
{ boolean: [ true, false ] }
```

3. 整数

```yaml
# 二进制表示
int:
  - 123
  - 0b1010_0111_0100_1010  # 二进制表示
```

```
{ int: [ 123, 42826 ] }
```

4. 浮点数

```yaml
float:
  - 3.14
  - 6.222221212e+5         # 可以使用科学计数法
```

```
{ float: [ 3.14, 622222.1212 ] }
```

5. null

```yaml
null:
  nodeName: 'node'
  parent: ~                # 使用 ~ 表示null
```

```yaml
{ null: { nodeName: 'node', parent: null } }
```

6. 日期

```yaml
date:
  - 2020-04-02             # 日期必须使用ISO 8601格式,即yyy-mm-dd
```

```yaml
{ date: [ Thu Apr 02 2020 08:00:00 GMT+0800 (中国标准时间) ] }
```

7. 时间

```yaml
datetime:
  - 2020-04-02T22:20:32+08:00  # 时间使用ISO 8601格式，时间和日期之间使用T连接，最后使用+代表时区
```

```yaml
{ datetime: [ Thu Apr 02 2020 22:20:32 GMT+0800 (中国标准时间) ] }
```

### 4.2.2 数组

（1）以`-` 开头的行表示构成一个数组

```yaml
- A
- B
- C
```

```yaml
[ 'A', 'B', 'C' ]
```

（2）多维数组：数据结构的子成员是一个数组

```yaml
-
  - A
  - B
-
  - A
  - B
```

```
[ [ 'A', 'B' ], [ 'A', 'B' ] ]
```

（3）复杂数组

```yaml
# workers属性是一个数组，每一个数组元素又是由id name salary 三个属性构成
workers:
  -
     id: 1
     name: lisi
     salary: 200W
  -
     id: 2
     name: zhangsan
     salary: 500W
```

```
{ workers: 
   [ { id: 1, name: 'lisi', salary: '200W' },
     { id: 2, name: 'zhangsan', salary: '500W' } ] }
```

### 4.2.3 对象

（1）对象键值对使用冒号结构表示`key:value`，冒号后面要加一个空格

```yaml
key:
  child-key1: value1
  child-key2: value2
```

也可以使用：

```yaml
{ key: { 'child-key1': 'value1', 'child-key2': 'value2' } }
```

（2）较复杂的对象格式

可以使用问号加一个空格代表一个复杂的key，配合一个冒号加一个空格代表一个 value

```yaml
# 意思即对象的属性是一个数组 [complexkey1,complexkey2],对应的值也是一个数组[value1,value2]
?
  - complexkey1
  - complexkey2
:
  - value1
  - value2
```

```yaml
{ 'complexkey1,complexkey2': [ 'value1', 'value2' ] }
```

## 4.3 引用和锚点

（1）` & ` 用来建立锚点，` << ` 表示合并到当前数据，` * ` 用来引用锚点

```yaml
defaults: &defaults
    adapter: postgres
    host: localhost
test:
    database: myapp_test
    <<: *defaults
```

```yaml
defaults: &defaults
    adapter: postgres
    host: localhost
test:
    database: myapp_test
    adapter: postgres
    host: localhost
```



# 5 Yaml-cpp api使用

## 5.1 安装

（1）cmake编译安装就是就是生成头文件及链接库文件。cmake安装重要的一点就是指定好安装的目录

1. 下载linux版本

```shell
# 注意下载的是linux版本后缀名:tar.gz, zip版本会报错
wget https://github.com/jbeder/yaml-cpp/archive/yaml-cpp-0.6.3.tar.gz
tar -zxvf yaml-cpp-0.6.3.tar.gz
cd yaml-cpp-yaml-cpp-0.6.3
```

2. 设置安装路径

```shell
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=/apps/sylar -DYAML_BUILD_SHARED_LIBS=ON ..
# ON 设置生成共享库
# cmake指定安装目录 cmake -DCMAKE_INSTALL_PREFIX=/apps/sylar
```

3. make && make install

```shell
# 库安装位置
-- Up-to-date: /apps/sylar/lib/libyaml-cpp.so.0.6.3
-- Up-to-date: /apps/sylar/lib/libyaml-cpp.so.0.6
-- Up-to-date: /apps/sylar/lib/libyaml-cpp.so
-- Up-to-date: /apps/sylar/include/yaml-cpp
-- Up-to-date: /apps/sylar/include/yaml-cpp/node
-- Up-to-date: /apps/sylar/include/yaml-cpp/node/detail
-- Up-to-date: /apps/sylar/include/yaml-cpp/node/detail/bool_type.h
-- Up-to-date: /apps/sylar/include/yaml-cpp/node/detail/impl.h
```



## 5.2 使用

### 5.2.1 基础用法

（1）node 是yaml-cpp 中重要的数据结构。Node一共有以下几种：

1. null 空节点 

2. Sequence序列，类似于一个vector，对应YAML格式中的数组

3. Map 类似标准库中的map，对应YAML格式中的对象

4. Scalar 标量，对应YAML格式中的常量

（2）读取文件

```c++
#include <yaml-cpp/yaml.h> // yaml-cpp 头文件
#include <iostream>

int main(int argc, char** argv) 
{
	try 
    {
		YAML::Node node = YAML::LoadFile("file.yml");
		std::cout << node << std::endl;              // 输出yaml数据
	} 
    catch (...)                                      // 文件为非yaml格式抛出异常
    {
		std::cout << "error" << std::endl;
	}
	return 0;
}

std::ifstream file("file.yaml");
YAML::Node node = YAML::Load(file); // 读取来自test.yaml的node文件
std::cout << node <<std::endl;
```

```yaml
# 输出结果
logs:
  - name: root
    level: info
    appenders:
      - type: FileLogAppender
        file: root.txt
      - type: StdoutLogAppender
  - name: system
    level: info
    appenders:
      - type: FileLogAppender
        file: system.txt
      - type: StdoutLogAppender
```



### 5.2.2 解析yaml

（1）总结

1. 基础用法如以上两个示例
2. 读取之前需要判断节点是否定义，IsDefined
3. 转换格式需要判断是否是对应的类型
4. Scalar()方法，对应string类型的节点类似于 as
5. 其他基本类型，可以使用as()等转化

```c++
#include <iostream>
#include <yaml-cpp/yaml.h>
#include <vector>

struct Appender
{
	std::string type;
	std::string file;
};

struct LogDefine
{
	std::string name;
	std::string level;
	std::vector<Appender> appenders;
};

int main(int argc, char** argv) 
{
	YAML::Node root = YAML::LoadFile("test.yml");
	if(!root["logs"].IsDefined()) {       // IsDefined() 判断节点是否存在
		std::cout << "logs is null" << std::endl;
		return 0;
	}
	if(!root["logs"].IsSequence()) {      // 因为logs是数组 IsSequence
		std::cout << "logs is not sequence" << std::endl;
		return 0;
	}
	
	std::vector<LogDefine> lds;          // sequence遍历方式 类似vector
	for(size_t i = 0; i < root["logs"].size(); ++i) {
		YAML::Node node = root["logs"][i];
		if(!node.IsMap()) {               // 判断是否是map结构,IsMap
			std::cout << node << " - not map" << std::endl;
			continue;
		}
		LogDefine ld;
		if(node["name"].IsDefined()) {
			if(node["name"].IsScalar()) {  // 判断是否简单类型,IsScalar
				ld.name = node["name"].IsScalar();
			}
            else {
				std::cout << "name not scalar" << std::endl;
			}
		}
		if(node["level"].IsDefined()) {
			if(node["level"].IsScalar()) {
				ld.name = node["level"].Scalar();
			} 
            else {
				std::cout << "level not scalar" << std::endl;
			}
		}
		
		if(node["appenders"].IsDefined() && node["appenders"].IsSequence()) {
			Appender a;
			for(size_t x = 0; x < node["appenders"].size(); ++x) {
				YAML::Node xnode = node["appenders"][x];
				if(xnode["type"].IsScalar()) {
					xnode.type = xnode["type"].Scalar();
				}
                else {
					std::cout << "type not scalar" << std::endl;
				}
				if(xnode["file"].IsScalar()) {
					xnode.type = xnode["file"].Scalar();
				} 
                else {
					std::cout << "file not scalar" << std::endl;
				}
				ld.appenders.push_back(a);
			}
		}
		lds.push_back(ld);
	}
}
```



### 5.2.3 转换成yaml格式

（1）Node可以使用文件流的方式进行读写，上面的std::cout已经证明了这一点。你可以这样保存一个node：

```c++
std::ofstream file("test.yaml");
file << node << std::endl;
```



```c++
#include <iostream>
#include <yaml-cpp/yaml.h>
#include <vector>

struct Appender
{
	Appender(const std::string& _type, std::string& _file) 
		:type(_type)
		,file(_file) {
	}
	std::string type;
	std::string file;
};

struct LogDefine
{
	LogDefine(const std::string& _name, const std::string& _level) 
		:name(_name)
		,_level(_level) {
	}
	std::string name;
	std::string level;
	std::vector<Appender> appenders;
};

int main(int argc, char** argv)
{
	std::vector<LogDefine> lds;
	{
		LogDefine ld1("root", "info");
		ld1.appenders.push_back(Appender("FileLogAppender", "./root.txt"));
		ld1.appenders.push_back(Appender("StdoutLogAppender", ""));
		lds.push_back(ld1);
			
		LogDefine ld2("system", "warn");
		ld2.appenders.push_back(Appender("FileLogAppender", "./system.txt"));
		ld2.appenders.push_back(Appender("StdoutLogAppender", ""));
		lds.push_back(ld2);
	}
	
	YAML::Node root;
	for(auto& i : lds)
    {
		YAML::Node node;
		node["name"] = i.name;
		node["level"] = i.level;
		
		for(auto& a : i.appenders)
        {
			YAML::Node anode;
			anode["type"] = a.type;
			if(!a.file.empty())
            {
				anode["file"] = a.file;
			}
			node["appenders"].push_back(anode);
		}
		root.push_back(node);
	}
	std::cout << root << std::endl;
	return 0;
}
```



