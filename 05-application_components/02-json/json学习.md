# JSON 学习

## 1 是什么？

1.  JSON（javascripts object notation） JSON 是存储和交换文本信息的语法 。类似xml，JSON比xml更小，更快，更易解析。这个 sites 对象是包含 3 个站点记录（对象）的数组。 

```json
{
    "sites":[
	{ "name":"菜鸟教程", "url":"www.runoob.com" },
	{ "name":"google", "url":"www.google.com" },
    { "name":"微博", "url":"www.weibo.com" }
    ]
}
```

2. JSON（javascripts object notation）javascripts对象表示法。JSON文本格式在语法上与创建JavaScript对象的代码相同。由于这种相似性，无需解析器，JavaScript 程序能够使用内建的 eval() 函数，用 JSON 数据来生成原生的 JavaScript 对象。 
3. JSON 轻量级的文本数据交换格式
4. JSON 使用 Javascript语法来描述数据对象，但是 JSON 仍然独立于语言和平台。JSON解析器和JSON库支持许多不同的编程语言
5. JSON具有自我描述性，更易理解。

## 2 用途

1. JSON 是存储和交换文本信息的语法 。类似xml

## 3 格式

### 3.1 语法

（1）JSON 语法是 JavaScript 对象表示语法的子集

```
- 数据在名称/值对中
- 数据由逗号分隔
- 大括号保存对象
- 中括号保存数组
```

（2）JSON名称/值对（JSON数据的书写格式）

1. 名称/值对包括字段名称（在双引号中），后面写一个冒号，然后是值： 

```json
"name" : "菜鸟教程"
```

2. 等价JavaScript语句

```javascript
name = "菜鸟教程"
```

（3）JSON值

1. 对象：对象在大括号{} 中书写，对象可以包含多个名称/值对

```json
{ "name":"菜鸟教程", "url":"www.runoob.com" }
```

```javascript
# 等价的JavaScript语句
name = "菜鸟教程"
url = "www.runoob.com"
```

2. 数组：JSON数组在中括号中书写，数组可包含多个对象

```json
# 对象"sites"包含三个对象的数组，每个对象代表一条关于网站(name 、 url)的记录
{
    "sites":[
	{ "name":"菜鸟教程", "url":"www.runoob.com" },
	{ "name":"google", "url":"www.google.com" }
    ]
}
```

3. 字符串

4. 数字

5. 逻辑值（true或false）

```json
{ "flag":true }
```

6. null

```json
{ "flag":null }
```

### 3.2 对象



### 3.3 数组





## 4 xml库的使用



## 5 联系

### 5.1 与JavaScript关系





### 5.2 与xml比较

1. JSON往往是更为简洁、快速，xml则更为严谨周全

   

```xml
<?xml version="1.0" encoding="UTF-8"?>
<city>
    <name>Beijing</name>
    <id>1</id>
</city>
```

```json
{
	"city":{
    "name":"Beijing",
    "id":1
    }
}
```



```xml
<?xml version="1.0" encoding="UTF-8"?>
<cities>
	<city>
    	<name>Beijing</name>
    	<id>1</id>
	</city>
    <city>
    	<name>Shanghai</name>
    	<id>2</id>
	</city>
</cities>
```



```json
{
	"city":[
    {"name":"Beijing","id":1},
    {"name":"Shanghai","id":2}
    ]
}
```





