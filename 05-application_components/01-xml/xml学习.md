xml学习

## 1 是什么？

### 1.1  定义

（1）xml本身是一种格式规范，是可扩展标记语言（eXtensible Markup Language），是一种包含了数据以及数据说明的文本格式规范。 

### 1.2 例子

（1）标记语言，可以理解为一种格式规范，文本结合文本相关的其他格式信息。

拿到一张报纸，我们马上就能分辨出来哪个是标题，哪些是段落，哪些是斜体，哪些是加粗……

现在我们需要把这张报纸做成电子版，那么问题来了：都是文字，电脑怎么知道哪个是标题，哪个是段落……

所以就有了标记语言HTML，要让电脑知道你放的某几个文字是个标题，就用`<h1></h1>`这么一对[标记]把标题文字包裹起来，这就完成了一次[标记]

电脑看到这段用一对标记包裹着的文字，就会为这段文字赋予对应的形态和功能。

而在电脑上，我们可以实现纸质版做不到的东西，比如超链接、插入视频和音频等。这些都用相应的标记来实现。

标记只是给电脑和程序员看的，用户看到的只是处理以后的内容，看不到标记。

这就是标记语言，用标记让你的文章更精彩！



（2）什么是文本文件：只有文本内容，没有任何格式。

举例：在记事本中输入“带你出师、闯荡江湖！”，并设置相应的格式，但是在其它电脑上打开并没有显示同样的格式，说明其不包含任何格式信息。查看文件大小显示20字节

在word中输入同样的内容，同时设置格式，在另一个电脑上看到的是同样的内容和同样的格式。同时查看文件的大小，说明word文档中除了存储内容，还存储了格式等信息（document.xml,）ml就是标记语言（Markup Language）的缩写。

我们要学的HTML（HyperText MarkedLanguage）也是一种标记语言。

超文本：不仅仅能表示文本信息，还能表示音视频、格式等等信息。

浏览网页实际上就是将存储在服务器上的HTML文件下载到本地，然后通过浏览器（IE、Chrome等）进行解析并呈现内容。



（3）数据都是一样的，不同的只是数据的格式，**xml本身是一种格式规范**，是一种包含了数据以及数据说明的文本格式规范

比如，我们要给对方传输一段数据，数据内容是 `too young,too simple,sometimes naive`，要将这段话按照属性拆分为三个数据的话，就是，年龄too young，阅历too simple，结果sometimes naive。

我们都知道程序不像人，可以体会字面意思，并自动拆分出数据，因此，我们需要帮助程序做拆分，因此出现了各种各样的数据格式以及拆分方式。

比如，可以是这样的

```
数据为“too young,too simple,sometimes naive”
然后按照逗号拆分，第一部分为年龄，第二部分为阅历，第三部分为结果。
```

也可以是这样的

```
数据为"too_young**too_simple*sometimes_naive"
从数据开头开始截取前面十一个字符，去掉号 * 并把下划线替换为空格作为第一部分，再截取接下来的十一个字符同样去掉 * 并替换下划线为空格作为第二部分，最后把剩下的字符同样去掉 * 号并替换下划线为空格作为第三部分。
```

这两种方式都可以用来容纳数据并能够被解析，但是不直观，通用性也不好，而且如果出现超过限定字数的字符串就容纳不了，也可能出现数据本身就是下划线字符导致需要做转义。

基于这种情况，出现了xml这种数据格式， 上面的数据用XML表示的话

可以是这样

```xml
<person age="too young" experience="too simple" result="sometimes naive" />
```

也可以是这样的

```xml
<person>
    <age value="too young" />
    <experience value="too simple" />
    <result value="sometimes naive" />
</person>
```

两种方式都是xml，都很直观，附带了对数据的说明，并且具备通用的格式规范可以让程序做解析。 

如果用json格式来表示的话，就是下面这样

```json
{
    "age":"too young",
    "experience":"too simple",
    "result":"sometimes naive"
}
```

看出来没，其实数据都是一样的，不同的只是数据的格式而已，同样的数据，我用xml格式传给你，你用xml格式解析出三个数据，用json格式传给你，你就用json格式解析出三个数据，还可以我本地保存的是xml格式的数据，我自己先解析出三个数据，然后构造成json格式传给你，你解析json格式，获得三个数据，再自己构造成xml格式保存起来，说白了，不管是xml还是json，都只是包装数据的不同格式而已，重要的是其中含有的数据，而不是包装的格式。



（4）编程语言 - 脚本语言 - 标记语言

编程语言（programming language）： 编译型语言写的程序执行之前，需要一个专门的编译过程，把程序编译成为机器语言的文件，比如exe文件，以后要运行的话就不用重新翻译了，直接使用编译的结果就行了（exe文件），因为翻译只做了一次，运行时不需要翻译，所以编译型语言的程序执行效率高。 

标记语言：标记语言不用于向计算机发出指令，常用于格式化和链接 

脚本语言：脚本语言介于标记语言和编程语言之间，脚本语言脚本语言不需要编译，可以直接用，由解释器来负责解释 


再说说它们的代表语言：

编程语言：C/C++，Java 等
标记语言：xml， html，  xhtml ( xml 和 html 的合体 )等，（可以看出它们都是以 "ml"尾的）
脚本语言：php，js，asp，Python，等



翻译的方式有两种，一个是编译，一个是解释。
解释类：应用程序源代码一边由相应语言的解释器“翻译”成目标代码（机器语言），一边执行，因此效率比较低，而且不能生成可独立执行的可执行文件，应用程序不能脱离其解释器，但这种方式比较灵活，可以动态地调整、修改应用程序

编译类：编译是指在应用源程序执行之前，就将程序源代码“翻译”成目标代码（机器语言），因此其目标程序可以脱离其语言环境独立执行，使用比较方便、效率较高。但应用程序一旦需要修改，必须先修改源代码，再重新编译生成新的目标文件（* .obj，也就是OBJ文件）才能执行，只有目标文件而没有源代码，修改很不方便。



（5）序列化与反序列化

序列化的原本意图是希望对一个对象作一下“变换”，变成字节序列，这样一来方便持久化存储到磁盘，避免程序运行结束后对象就从内存里消失，另外变换成字节序列也更便于网络运输和传播，所以概念上很好理解 。

- **序列化**：把Java对象转换为字节序列。
- **反序列化**：把字节序列恢复为原先的Java对象

 ![序列化与反序列化  ](pic\2-xml序列化.jpg) 

## 2 用途

1.  XML 被设计用来传输和存储数据。
2. 从应用来讲，各种**配置文件**文件是xml的一个重要应用。
3. 数据文件也能用xml来保存，比如office文件。
4. 另外，**SOAP协议**的载体也是基于XML；
5. ATOM也是基于XML用来表达要传输的数据，虽然现在json用得更多点。 

## 3 格式

### 3.1 树形结构

（1）从这个实例中，XML 文档包含了一张 Jani 写给 Tove 的便签。XML 具有出色的自我描述性，您同意吗？

```xml
<?xml version="1.0" encoding="UTF-8"?>
<note>
	<to>Tove</to>
	<from>Jani</from>
	<heading>Reminder</heading>
	<body>Don't forget me this weekend!</body>
</note>
```

1.  第一行是 XML 声明。它定义 XML 的版本（1.0）和所使用的编码（UTF-8 : 万国码, 可显示各种语言）
2.  下一行描述文档的根元素（像在说："本文档是一个便签"） 

（2）XML 文档必须包含根元素。该元素是所有其他元素的父元素。XML 文档中的元素形成了一棵文档树。这棵树从根部开始，并扩展到树的最底端。父、子以及同胞等术语用于描述元素之间的关系。父元素拥有子元素。相同层级上的子元素成为同胞（兄弟或姐妹）。所有的元素都可以有文本内容和属性（类似 HTML 中）。

### 3.2 语法

（1）XML 必须包含根元素，它是所有其他元素的父元素，且根标签只有一个

（2）XML 声明文件的可选部分，如果存在需要放在文档的第一行

```xml
UTF-8 也是 HTML5, CSS, JavaScript, PHP, 和 SQL 的默认编码
<?xml version="1.0" encoding="UTF-8"?>
```

（3）所有的xml元素都必须有一个关闭标签，在 HTML 中，某些元素不必有一个关闭标签 

```xml
<p>This is a paragraph.</p>
<br />
```

（4）xml标签对大小写敏感

```xml
<Message>这是错误的</message>
<message>这是正确的</message>
```

（5）xml必须正确嵌套

1. 在xml中所有元素都必须彼此正确的嵌套

```xml
<b><i>This text is bold</i></b>
```

2. 在 html 中，常会看到没有正确嵌套的元素

```html
<b><i>This text is bold and italic</b></i>
```

（6）xml属性值必须加引号。第一个是错误的，第二个是正确的

```xml
<note date=6/25/2020>                   # note 元素中的 date 属性没有加引号
<to>Tom</to>
<from>Jani</from>
</note>
```

```xml
<note date="6/25/2020">
<to>Tom</to>
<from>Jani</from>
</note>
```

（7）xml中的注释，在 xml中编写注释的语法与 html的语法很相似。 

```xml
<!-- This is a comment -->
```

（8）实体引用，如果您把字符 "<" 放在 XML 元素中，会发生错误，这是因为解析器会把它当作新元素的开始。

```xml
<message>if salary < 1000 then</message>
```

1. 为了避免这个错误，请用实体引用代替"<" 字符

```
<message>if salary &lt; 1000 then</message>
```

2.  在 XML 中，有 5 个预定义的实体引用： 

| 引用     | 符号 | 含义           |
| -------- | ---- | -------------- |
| `&lt;`   | <    | less than      |
| `&gt;`   | >    | greater than   |
| `&amp;`  | &    | ampersand      |
| `&apos;` | '    | apostrophe     |
| `&quot;` | "    | quotation mark |

（9）在xml中，空格会被保留， html 会把多个连续的空格字符裁减（合并）为一个 

![xml中空格会被保留](pic\1-xml中空格会被保留.png)

10. xml以LF（换行符）存储换行

在 Windows 应用程序中，换行通常以一对字符来存储：回车符（CR）和换行符（LF），在 Unix 和 Mac OSX 中，使用 LF 来存储新行； XML 以 LF 存储换行

### 3.3 元素

（1）xml 元素指的是从（且包括）开始标签直到（且包括）结束标签的部分。

1. 一个元素可以包含：其他元素，文本，属性，或混合以上所有。`<bookstore>`有元素内容，`<book> `元素也有属性(category="C++")，`<title>、<author> 、 <price>`有文本内容。

```xml
<bookstore>
	<book category="C++">
        <title>Learning C++</title>
        <author>xujinhui</author>
        <price>100</price>
	</book>
</bookstore>
```

（2）命名规则：名称不能包含空格，不能以xml（XML等）开始。可使用任何名称，没有保留字词。

（3）命名习惯

```
使名称具有描述性。使用下划线的名称也很不错：<first_name>、<last_name>。
名称应简短和简单，比如：<book_title>，而不是：<the_title_of_the_book>。
避免 "-" 字符。如果您按照这样的方式进行命名："first-name"，一些软件会认为您想要从 first 里边减去 name。
避免 "." 字符。如果您按照这样的方式进行命名："first.name"，一些软件会认为 "name" 是对象 "first"的属性。
避免 ":" 字符。冒号会被转换为命名空间来使用（稍后介绍）。
XML文档经常有一个对应的数据库，其中的字段会对应 XML 文档中的元素。有一个实用的经验，即使用数据库的命名规则来命名 XML 文档中的元素。
```

（4）元素可扩展，让我们设想一下，

1. 我们创建了一个应用程序，可将 `<to>、<from>以及 <body> `元素从 XML 文档中提取出来，产生以下输出：

```xml
<note>
    <to>Tove</to>
    <from>Jani</from>
    <body>Don't forget me this weekend!</body>
</note>
```

```
MESSAGE
To: Tove
From: Jani
```

2. xml文档的作者添加的一些额外信息

```xml
<note>
    <date>2008-01-10</date>
    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
</note>
```

那么这个应用程序会中断或崩溃吗？不会。这个应用程序仍然可以找到 XML 文档中的` <to>、<from> 以及 <body> `元素，并产生同样的输出。XML 的优势之一，就是可以在不中断应用程序的情况下进行扩展

### 3.4 属性

（1）xml元素具有属性，类似 html。属性（Attribute）提供有关元素的额外信息。属性通常不提供数据组成部分的信息。在下面的实例中，文件类型与数据无关，但是对需要处理这个元素的软件来说却很重要。

```xml
<file type="gif">computer.gif</file>
```

（2）属性值必须被引号包围，单引号和双引号均可使用。如果属性值本身包含双引号，可以使用单引号，也可以使用字符实体。

```xml
<animals name='"dog" "cat"'>
```

（3）xml元素 vs 属性

```xml
<note date="10/01/2020">
<to>Tim</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>Don't forget me this weekend!</body>
</note>
```

```xml
<note>
<date>10/01/2008</date>
<to>Tim</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>Don't forget me this weekend!</body>
</note>
```

在第一个实例中，date是一个属性。在第二个实例中，date 是一个元素。这两个实例都提供相同的信息。

没有什么规矩可以告诉我们什么时候该使用属性，而什么时候该使用元素。我的经验是在 HTML 中，属性用起来很便利，但是在 XML 中，您应该尽量避免使用属性。如果信息感觉起来很像数据，那么请使用元素吧。

```xml
<note>
<date>
<day>10</day>
<month>01</month>
<year>2008</year>
</date>
<to>Tim</to>
<from>Jani</from>
<heading>Reminder</heading>
<body>Don't forget me this weekend!</body>
</note>
```

（4）避免使用属性，这不是xml应该被使用的方式。

```
<note day="10" month="01" year="2008"
to="Tove" from="Jani" heading="Reminder"
body="Don't forget me this weekend!">
</note>
```

属性不能包含多个值（元素可以），属性不能包含树结构（元素可以），属性不容易扩展（为未来的变化），属性难以阅读和维护，尽量使用元素来描述数据。而仅仅使用属性来提供与数据无关的信息 

## 4 xml使用

### 4.1 安装

（1）解析xml开源库：minixml、tinyxml、pugixml、mxml

（2）minixml安装

```shell
1 下载mxml-3.1.tar.gz
https://github.com/michaelrsweet/mxml/releases

2 解压
tar -xvzf mxml-3.1.tar.gz -C /home/

3 minixml安装配置路径
./configure --prefix=/foo (指定安装路径,默认路径/usr/local)
./configure --enable-threads=no

4. 构建源代码，用来测试程序能否正常运行
make
5.安装
make install
Installing libmxml.so to /usr/local/lib... 数据拷贝

6.关联库
由于如果没有root权限，无法将库文件放入默认库目录文件，目前解决方法是设置环境变量，输入：
export LIBRARY_PATH=/usr/local/lib (安装路径下的lib目录)
export LD_LIBRARY_PATH=/usr/local/lib (安装路径下的lib目录)

7.关联头文件
将安装目录下的mxml.h放入xml.c（自己写的解析xml程序）目录下
安装好之后，就可以体验解析xml的欢乐了，之后我对xml进行了解析，创建，和修改

8 测试
gcc testmxml.c -lmxml -lpthread -o testmxml
./testmxml test.xml

包含头文件 mxml.h
编译的时候需要添加动态库: libmxml.so (/usr/local/lib)
```

### 4.2 解析xml文件

（1）加载xml文件到内存

```c
mxml_node_t *mxmlLoadFile(
	mxml_node_t *top,                    // 一般为NULL，从哪一处开始解析
	FILE *fp,                            // 文件指针
	mxml_type_t(*cb)(mxml_node_t *)      // 默认MXML_NO_CALLBACK
)
```

（2）获取节点的属性

```c
const char *mxmlElementGetAttr(
	mxml_node_t *node,                   // 带属性的节点的地址
	const char *name                     // 属性名
)	
```

（3）获取指定节点的文本内容  

```c
const char *mxmlGetText(
	mxml_node_t *node,                   // 节点的地址
	int *whitespace                      // 是否有空格
)
```

（4）跳转到下一节点  

```c
mxml_node_t *mxmlWalkNext(
	mxml_node_t *node,                   // 当前节点
	mxml_node_t *top,                    // 根节点
	int descend
)
descend:搜索的规则
MXML_NO_DESCEND：查看同层级
MXML_DESCEND_FIRST：查看下一层级的第一个
MXML_DESCEND：一直向下搜索
```

（5）查找节点   

```c
mxml_node_t *mxmlFindElement (
	mxml_node_t *node,             	     // 当前节点
	mxml_node_t *top,              		 // 根节点
	const char* name,              		 // 查找的标签名
	const char* attr,                    // 查找的标签的属性,没有属性传NULL
	const char *value,                   // 查找标签的属性值
	int descend                          // 同上
)
```



### 4.3 mxml生成xml文件

（1）创建一个新的xml文件  

```c
mxml_node_t *mxmlNewXML(const char *version)
返回新创建的xml文件节点
默认的文件的编码为utf8
```

（2）删除节点内存

```c
void mxmlDelete(mxml_node_t* node)
```

（3）添加一个新的节点

```c
mxml_node_t *mxmlNewElement(
	mxml_node_t* parent,         // 父节点
	const char* name             // 新节点标签名
);
```

（4） 设置节点的属性名和属性值  

```c
void mxmlElementSetAttr (
	mxml_node_t* node,           // 被设置属性的节点
	const char* name,            // 节点的属性名
	const char* value            // 属性值
);
```

（5）创建节点的文本内容

```c
mxml_node_t *mxmlNewText (
	mxml_node_t* parent,         // 节点地址
	int whitespace,              // 是否有空白0
	const char* string           // 文本内容
);
```

（6）保存节点到xml文集那

```c
int mxmlSaveFile (
	mxml_node_t* node,           // 根节点-头节点
	FILE *fp,                    // 文件指针(open && 文件具有写权限)
	mxml_save_cb_t cb            // 默认MXML_NO_CALLBACK
);
```

（7）例子

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <mxml.h>

int main(int argc, char * argv [ ])
{
	mxml_node_t *xml = mxmlNewXML("1.0");              // 文件头 xml
	mxml_node_t *china = mxmlNewElement(xml, "china"); // 根标签 china
	/*********************北京***********************/
	mxml_node_t *city = mxmlNewElement(china, "city"); // 子标签 city
	mxml_node_t *info = mxmlNewElement(city, "name");  // info
	mxmlNewText(info, 0, "beijing");                   // 标签赋值
	mxmlElementSetAttr(info, "isbig", "Yes");          // 设置属性
    
	mxml_node_t *area = mxmlNewElement(city, "area");  // area
	mxmlNewText(area, 0, "16410");

	mxml_node_t *population = mxmlNewElement(city, "population"); // population
	mxmlNewText(population, 0, "2171");

	mxml_node_t *GDP = mxmlNewElement(city, "GDP");    // GDP
	mxmlNewText(GDP, 0, "24541");

	/*********************南京***************************/
    city = mxmlNewElement(china, "city");
	info = mxmlNewElement(city, "name");               // info
	mxmlNewText(info, 0, "nanjing");                   // 标签赋值
	mxmlElementSetAttr(info, "isbig", "No");           // 设置属性

	area = mxmlNewElement(city, "area");               // area
	mxmlNewText(area, 0, "2188");

	population = mxmlNewElement(city, "population");   // population
	mxmlNewText(population, 0, "1860");

	GDP = mxmlNewElement(city, "GDP");                 // GDP
	mxmlNewText(GDP, 0, "21111");

	FILE* fp = fopen("china.xml", "w");
	mxmlSaveFile(xml, fp, MXML_NO_CALLBACK);
	fclose(fp);
	mxmlDelete(xml); 							       // 后序遍历
	return 0;
}
```

![mxml的使用](pic\3-mxml的使用.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<china>
    <city>
        <name isbig="Yes">beijing</name>
        <area>16410</area>
        <population>2171</population>
        <GDP>24541</GDP>
    </city>
    <city>
        <name isbig="No">nanjing</name>
        <area>2188</area>
        <population>1860</population>
        <GDP>21111</GDP>
    </city>
</china>
```



## 5 联系

### 5.1 与html比较



### 5.2 与Json比较