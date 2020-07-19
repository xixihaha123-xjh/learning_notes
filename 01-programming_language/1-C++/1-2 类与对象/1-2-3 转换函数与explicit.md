# C++类与对象

## 1 conversion function 转换函数

1. class Fraction的对象可不可以转换成另一个对象/变量？另一个对象/变量可不可以转换成class Fraction 的对象 ？

```c++
class Fraction
{
public:
	Fraction(int num, int den = 1) : _num(num), _den(den) {}
    // Fraction可以转换成double，编译器将Fraction转换为double时，调用此函数，转换不能有参数
    // 无返回值（operator后边已说明）
	operator double() const   // 转换不能改动Fraction的数据，所以转换函数通常加const 
	{ return (double) _num / _den; }
private:
	int _num;  // 分子
	int _den;  // 分母
};
// class Fraction的对象转换为double
{
	Fraction f(3, 5);
	double d = 4 + f;  // 调用operator double()将f转换为0.6
}
```

## 2 单参数的构造函数

1. 另一个对象/变量可不可以转换成class Fraction的对象 ？

```c++
// double变量转换为class Fraction对象
class Fraction
{
public:
	// 单参数构造函数，one argument，意思是给一个参数就够了
    Fraction(int num, int den = 1) : _num(num), _den(den) // 单参数构造函数
	{   cout << "class Fraction ctor\n"; }

	Fraction operator+(const Fraction& f)
	{
		cout << "function Fraction operator+ \n";
		return f;
	}

private:
	int _num;  // 分子
	int _den;  // 分母
};
// 调用
{
	Fraction f(3, 5);
	Fraction d = f + 4; //调用non-explicit ctor将4转为Fraction(4, 1) 然后调用 operator+
// 执行结果
class Fraction ctor
class Fraction ctor
function Fraction operator+
```

2. 参数转换 vs 单参数构造函数

```c++
class Fraction
{
public:
	Fraction(int num, int den = 1) : _num(num), _den(den)
	{   cout << "class Fraction ctor\n"; }
    // 转换函数
	operator double() const   
	{   return (double) _num / _den; }
    // + 号操作符重载
	Fraction operator+(const Fraction& f)
	{
		cout << "function Fraction operator+ \n";
		return f;
	}
private:
	int _num;  // 分子
	int _den;  // 分母
};
// 调用
{
	Fraction f(3, 5);
	Fraction d = f + 4;
}
// 执行结果 -- error 歧义 
// 4 可以转换成Fraction对象（通过构造函数），转换函数可以将Fraction对象转换为double
```

![执行结果](F:\00-笔记重要\02-语言学习\Pic\C++\32单参数构造函数.png)



3. explicit - one-arguments ctor

* explicit 明白的，明确的，告诉编译器你不要给我暗度陈仓，不要给我自动的做事情，我既然设计是构造函数，我就以构造函数的事情，帮我调用它

```c++
class Fraction
{
public:
	explicit Fraction(int num, int den = 1) : _num(num), _den(den)
	{   cout << "class Fraction ctor\n"; }
    // + 号操作符重载
	Fraction operator+(const Fraction& f)
	{
		cout << "function Fraction operator+ \n";
		return f;
	}
private:
	int _num;  // 分子
	int _den;  // 分母
};
// 调用
{
	Fraction f(3, 5);
	Fraction d = f + 4;
}
// 执行结果
不存在从 "double" 转换到 "Fraction" 的适当构造函数
```



4. 转换函数在标准库中的应用

![转换函数在标准库中的应用](F:\00-笔记重要\02-语言学习\Pic\C++\33转换函数在标准库中的应用.png)

* vector<bool, Alloc> 模板偏特化，意思是vector中存放的是bool元素。operator[] 取出vector中的元素，返回值应该是一个bool值，但代码中返回的是一个reference，这个设计手法上叫代理（用reference代理bool）。所以reference类中，就必须要有一个转换函数。用A去代表B，A中就必须要有一个转换函数，将A转换为B



## 3 explicit

### 3.1 C++2.0 之前

1. 2.0 之前 防止单参数的构造函数进行隐式类型转换  

```c++
struct Complex
{
	int real, imag;
	Complex(int re, int im = 0) :real(re), imag(im) { }
	
    Complex operator+(const Complex& x)
	{ return Complex((real + x.real), imag + x.imag); }
};

Complex c1(12, 5);
Complex c2 = c1 + 5;   // 将 5 转换为 5+0i

// 加 explicit
explicit Complex(int re, int im = 0) :real(re), imag(im)
Complex c1(12, 5);
Complex c2 = c1 + 5;
二进制“+”:没有找到接受“int”类型的右操作数的运算符(或没有可接受的转换)
```

2. explicit（明白地，明确地）这个关键字用途比较少，主要用于构造函数中。比如说上例：不要编译器像左边那样自动把`5`转换为`5+0i`，就加上explicit关键字，这样编译器遇到`+5`时便会报错，就是说你不要自动地帮我调用，我要明确地使用

### 3.2 C++2.0 之后

1. 防止多参数的构造函数进行隐式类型转换  

```c++
class P {
public:
	P(int a, int b) {  cout << "P(int a, int b) \n";  }
	P(initializer_list<int>) {  cout << "P(initializer_list<int>) \n";  }
	explicit P(int a, int b, int c) { cout << "explicit P(int a, int b, int c) \n";  }
};

void fp(const P&) {  };

int main()
{
	P p1(77, 5);                            // P(int a, int b)
	P p2{ 77, 5 };                          // P(initializer_list<int>)
	P p3{ 77, 5, 42};                       // P(initializer_list<int>)
	P p4 = { 77, 5 };                       // P(initializer_list<int>)
	P p5 = { 77, 5, 42 };                   // P(initializer_list<int>)
	P p6(77, 5, 42);                        // explicit P(int a, int b, int c)
	
	fp({ 77, 5 });                          // P(initializer_list<int>)
	fp({ 77, 5, 3 });                       // P(initializer_list<int>)
	fp( P{ 77, 5 });                        // P(initializer_list<int>)
	fp(P{ 77, 5, 3 });                      // P(initializer_list<int>)

	P p11{ 77, 5, 32, 5000 };               // P(initializer_list<int>)
	P p12 = { 77, 5, 32, 5000 };            // P(initializer_list<int>)
	P p13{ 10 };                            // P(initializer_list<int>)
}
```

