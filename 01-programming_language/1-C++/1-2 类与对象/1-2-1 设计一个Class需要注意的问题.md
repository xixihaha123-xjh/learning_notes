# 设计一个Class需要注意的问题

## 1 设计一个class需要注意的5点

1. 成员变量数据放在private作用域，成员函数放在public作用域（单例设计模式例外）
2. 构造函数，使用初始化列表方式初始化成员变量
3. 类的body内，对于不改变类的成员变量的函数，尽可能使用const修饰
4. 参数的传递尽量考虑 pass by reference，需不需要加const视情况而定
5. 函数返回值，考虑返回pass by value 还是 pass by reference，尽可能使用pass by reference



## 1 成员变量数据放在private作用域，成员函数放在public作用域

1. C++ 的 private 用于封装，限制成员变量及函数的可见性
2. 设计模式 - 单例模式 Singleton：构造函数声明为private - 不允许被创建对象

```c++
// 定义
class Singleton 
{
public:
	static Singleton& getInstance()
	{
		static Singleton instance;
		return instance;
	}
	void printTest()
	{
		cout << "do something" << endl;
	}
private:
	Singleton() {}//防止外部调用构造创建对象
	Singleton(Singleton const &singleton);//阻止拷贝创建对象
	Singleton& operator=(Singleton const &singleton);//阻止赋值对象
};
// 使用
Singleton &a = Singleton::getInstance();
a.printTest();
```

3. 单例模式细节详见 - 模式设计：单例模式

## 2. 构造函数，使用初始化列表方式初始化成员变量

1. 初始化列表方式相当于初始化成员变量，比成员变量赋值的效率高

```c++
// 初始化列表方式相当于初始化成员变量 - 比成员变量赋值的效率高
complex(double r = 0, double i = 0) : re(r), im(i) // initialization list
{ }
```

```c++
complex(double r = 0, double i = 0)
{
	re = r; im = i;                                // 成员变量赋值
}
```

3. 初始化列表 initialization list 应用

* {  } 初始化 、构造函数初始化成员变量、C++2.0详细讲解

## 3 常量成员函数

1. 类的body内，对于不改变类的成员变量的函数，尽可能使用const修饰

```c++
class complex
{
public:
	...
	inline double real() const { return re; } // 不改变类的成员变量的函数，可使用常量成员函数
	inline double imag() const { return im; }
private:
	double re, im;
};
```

2. 为什么尽可能使用const修饰类的成员函数？

```c++
const complex c1(2, 1); // const修饰对象，表示对象的内容不能被更改，
cout << c1.real();     
cout << c1.imag();  
// real函数与imag函数不加const，编译器不会报错，但在类的设计上
// const complex c1; 表示c1这个复数对象的实部与虚部不能改变
// 不加const的成员函数double real(); 表示real()这个成员函数可以改变实部的值，与const complex c1相矛盾
```

## 4.参数的传递尽量考虑 pass by reference，需不需要加const视情况而定

1. 参数值传递与引用传递

* 值传递 - 函数压栈，产生临时对象，进行数据的拷贝，影响程序的效率。如果传递的参数比较小，例如char类型，可以用值传递
* 引用的底层实现利用了指针，参数传递pass by reference 仅传递指针大小，速度快，效率高
* pass by reference - to const

```c++
ostream& operator << (ostream& os, const complex& x) // to const 不希望被修改
{
	return os << '(' << x.real << ',' << x.imag << ')';
}
```



## 5 函数返回值，考虑返回pass by value 还是 pass by reference

1. 什么情况下可以return by reference，什么情况下 return by  value？

* 返回值不是在函数中临时创建的，而是原本就有的，return by reference



## 6. 设计一个class的过程

### 6.1 防卫式的宏定义

```c++
#ifndef __COMPLEX__
#define __COMPLEX__
#endif // !__COMPLEX__
```

### 6.2 设计一个complex类

```c++
class complex
{
}
```

1. 考虑复数需要具备什么样的数据，数据放在private 作用域

2. 考虑我要实现哪些功能，写哪些函数 ：

* 构造函数：构造函数参数要不要默认值，参数的传递pass by value / reference，initialization list 初值列表、构造函数还需要做什么事情
* 我要支持 什么样的操作，设计哪些函数：比如：+= 、-= 、 设计为成员函数还是非成员函数；
* 设计函数需要考虑：参数传递、函数返回类型如何定义、函数要不要加const（在real() imag() 不修改成员数据，因此加const）

```c++
// 第一个参数会被改动，第二个参数不会被改动
inline complex& __doapl(complex* this, const complex& r)
{
    this->re += r.re;
    this->im += r.im;
    return *this;
}
```

* 内联函数：在类内定义的函数，便自动成为内联函数的候选人，没有在类内定义的函数，也可声明为内联函数

```c++
class complex
{
public:
	complex(double r = 0, double i = 0) :re(r),im(i) {}  // 成员函数初始化列表
	complex& operator +=(const complex&);                // 返回值引用类型，参数引用类型加const
	inline double real() const { return re; } //在real() imag() 不修改成员数据，因此加const
	inline double imag() const { return im; }
private:               
	double re, im;
    
    friend complex& __doapl(complex*, const complex&);
};


#endif // !__COMPLEX__
```



### 6.3  operator +=

* 在class外 

```c++
//    两个对象相加： C++可以通过操作符操作实现两个复数相加
// 1. 由于 operator+= 是一个成员函数，作用域为complex::
// 2. complex::operator+= 是一个成员函数，+= 操作符作用在左边对象，左边对象，作为一个隐藏的对象，放在
//    this 的位置，编译器不同，this位置也不同，+=操作符重载函数参数只写右边就行
//    之后考虑参数是pass by reference 还是pass by value，还要考虑参数要不要加const
// 3. 考虑返回值：+= 右边的对象加到左边对象上，返回值为 += 左边对象的类型
//    返回值要不要加 reference，取决于传递出去的东西是不是临时对象，
//    也就是说不是在 += 函数中创建出来的，那么返回值就可以传递引用
//    += 左边对象本身就是存在的 this，所以可以传引用
// 4. 最后函数是在class外，加上inline,建议编译器编译为内联函数，但最后是不是内联函数不一定
// 5. 函数执行的动作
inline complex& complex::operator += (const complex& r)
{
	this->re += r.re;
	this->im += r.im;
	return *this;
}
// 相当于：
inline complex& complex::operator += (this, const complex& r) // 编译器不同，this位置不同
{
	this->re += r.re;
	this->im += r.im;
	return *this;
}
// 调用：
{
    complex c1(2, 1);
    complex c2(5);
    c2 += c1;      // 二元操作符 += 作用于C2,也就是下面complex::operator += 函数参数隐藏的this
}
```



### 6.4 operator +

* class 的非成员函数 - 也就是全局函数，没有this pointer

```c++
// 1. 为什么要把 operator+ 这个函数设计为非成员函数？
//    复数可以加实数，实数也可以加复数有三种变化，设置为成员函数，功能会受限
//    operator +设计为成员函数只能应付复数加复数，不能应付复数加实数或者实数加复数
//    (设置为成员函数，有一个隐藏的this，编译器提示此运算符参数过多)
// 2. operator+ 为全局函数，没有complex:: 作用域
// 3. + 二元操作符，参数分为左边与右边也就是x与y， +号不会改变左边/右边的值，所以函数参数设置为const，
//    pass by reference 可以提高程序效率，所以参数为引用类型
// 4. +号左边与右边 加完之后，产生新的对象，所以返回值不能为 reference
//    产生的新东西，不能放在左边x身上，也不能放在右边y身上，必须有一个新对象存放操作结果
//    所以操作结果存放在 operator+ 函数内部新创建的一个 local object上，所以返回值一定不能为引用
//    operator+ 函数调用完后，操作结果被销毁，如果是返回值为引用类型，返回的结果未知
// 5. 全局函数设置为 inline
complex operator + (const complex& x, const complex& y)
{
	return complex(x.real() + y.real(),
		           x.imag() + y.imag() );
}
// 6. 函数执行操作可以在函数内创建一个对象，用于存放操作结果
//    也可以是一个临时对象，创建一个临时对象，类型()，临时对象可以存放初值

// 7. operator + 还有两种操作方法
complex operator + (const complex& x, double y)  // 复数 + double
{
	return complex(x.real() + y, x.imag());
}

complex operator + (double x, const complex& y)  // double + 复数
{
	return complex(x + y.real(), y.imag()); // 返回一个临时对象，函数返回后其生命周期结束
}
```



### 6.5 重载正负号

```c++
// 1. 编译器根据参数的个数， 判断是正负号还是加号
inline complex operator + (const complex& x)
{ return x; }
// 2. + 号没有产生新的结果，不用创建临时对象，返回值类型可以为reference
inline complex operator - (const complex& x)
{ return complex(-x.real(), -x.imag(); ) }
// 3. - 号不可return by reference，因为其返回的必定是个local object
// 4. complex(-x.real(), -x.imag(); ) 创建临时对象
```



### 6.6 操作符重载 - cout



```c++
// 1. 将复数丢到cout上输出到屏幕，操作符重载一定是作用在左边的操作数身上，c1 << cout;
//    但是 c1 << cout;这样写完全违背使用者过去的操作习惯（对象放在 << 号右边，cout放在 << 号左边）
//    所以 c1 << cout;这样写要避免，操作符重载有成员函数与非成员函数，cout << c1;只能写成非成员函数
// 2. 成员名：operator << 
complex c1(2, 1);
cout << c1;
c1 << cout; // error写法
// 3. 参数类型：<< 号，左边cout是 ostream 类型，右边 c1 是complex类型
// 4. 参数
//    ostream可以是一包东西，占用字节数大所以是引用。右边c1东西丢到左边cout上，不改变c1，使用是 const
//    cout的状态在函数中一直在改变，所以参数 os不能是const
// 5. 返回值
//    如果cout 仅输出一个对象 cout << c1; c1 丢给cout就结束了，返回值可以设计为void
//    使用者如果连续输出，如cout << c1 << endl;就需要返回 cout类型的东西，用于接收更右边的对象用于输出
//    ostream不是 operator << 函数的临时对象，它是本来就有的东西，所有返回引用类型 ostream&
ostream& operator <<(ostream & os, const complex& c1)
{
    return os << '(' << x.real() << ',' << x.imag() << ')';
}
```



