# C++面向对象开发

### 2、头文件与类的声明

C++中class与struct几乎没什么区别

class的两种经典分类

（1）Object Based：不带指针的类

（2）Object Oriented： 带指针的类

模板

3、

内联函数 inline ，内联函数更快，但是是否能变成内联函数由编译器决定，不是在class内定义的函数都是内联函数

构造函数也可以重载（overloading），重载实际上是编译器给函数取了不同的名字 

如果把构造函数放在private，则该类不可被构造。

对于不会改变数据的函数，后面加const，如：

```c++
double real() const { return re; }
double imag() const { return im; }
```

函数传参与返回都尽量传引用

同一个class的各个对象互为友元

临时对象，class（）

堆（stack）栈（heap）

```
直接构造，对象放在栈中，生命周期为其作用域
使用new构造，对象放在堆中，生命周期为全局，需要手动delete释放内存
```

静态函数无this指针，只能处理静态数据

类模板，函数模板 template<>



##### 类之间的关系

**构造由内而外，析构由外而内**。

复合：对象中包含对象

委托（Delegation）：对象中包含对象指针

继承：数据被继承，函数的调用权被继承



非虚函数 ：不希望子类重载此函数

虚函数（virtual）：希望子类能重载此函数，并且此函数有默认定义

纯虚函数：希望子类一定要重载此函数，此函数无默认定义

```c++
class Shape
{
public:
	virtual void draw() const = 0;  //纯虚函数
	virtual void error(const std::string& msg); //虚函数
	int objectID() const; //非虚函数
}
```



# 面向对象（II）

### 转换函数

转换函数即conversion function，主要用于对象之间的转换。

```c++
class Fraction
{
public:
    explicit Fraction(int num, int den=1) : m_numberator(num), m_denominator(den) {}

    // 转换函数，可将此对象转换为double
    operator double() const
    {
        return ( (double) m_numberator / m_denominator);
    }
private:
    int m_numberator;  // 分子
    int m_denominator; // 分母
};
```

其中的`operator double()`就是转换函数，**这个函数不需要写返回值类型**

```c++
 Fraction f(3, 5);
 double d = 4 + f; // 将f转换为double类型的0.6，
```

当然，也可以将其他类型转换为`Fraction`，如：

```c++
class Fraction
{
public:
    Fraction(int num, int den=1) : m_numberator(num), m_denominator(den) {}
    
    Fraction operator+ (const Fraction& f)
    {
        return Fraction(m_numberator + f.m_numberator, m_denominator + f.m_denominator);
    }

private:
    int m_numberator;  // 分子
    int m_denominator; // 分母
};
```



```c++
Fraction f(3, 5);
Fraction d2 = f + 4; //调用non-explicit ctor，将4转换为Fraction(4, 1)
```

`explicit`只能用于修饰只有一个参数的类的构造函数（或者只有第一个参数没给定默认值，其他参数都有默认值的构造函数），其作用是表明该构造函数是显式的，而不是隐式的，与其对应的是`implicit`，类的构造函数默认声明为`implicit`，即隐式。

```c++
Fraction d = 4;
```

在C++中，如果构造函数只有一个参数（或者只有第一个参数没给定默认值，其他参数都有默认值），那么在编译的时候就会有一个缺省的转换操作：将该构造函数对应数据类型的数据转换为该类对象，因此，上述的代码会将int类型的4隐式转换为Fraction。`explicit`的作用就是防止类构造函数的隐式自动转换，用法如下：

```c++
class Fraction
{
public:
    explicit Fraction(int num, int den=1) : m_numberator(num), m_denominator(den) {}
};
```



### 智能指针

> https://www.cnblogs.com/tenosdoit/p/3456704.html

> https://blog.csdn.net/sjp11/article/details/123899141

使用智能指针是为了更好地进行内存管理，自动delete释放对象的内存。c++中有四个智能指针: auto_ptr, shared_ptr, weak_ptr, unique_ptr 其中后三个是c++11支持，并且第一个已经被c++11弃用。

使用智能制造需要包含头文件`#include <memory>`

#### 迭代器

迭代器本质上可以看作智能指针

### 仿函数

重载小括号运算符的类



### 模板特化

全特化：指定所有的模板类别

```c++
template<>
class A<int, char>
{

};
```

偏特化：指定部分模板类别，一定是从前往后开始

```c++
template<typename T>
class A<int, T> //第一个是int，后面随意
{

};
```

```c++
template<typename T1, typename T2>
class A<T1*, T2*> //指定是指针类型，具体什么指针随意
{

};
```

